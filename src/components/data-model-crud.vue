<script>
//post id တစ်ခုချင်းဆီအတွက် unique id ထုတ် ေပးမယ့် function
function uniqueId() {
  return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
    (
      c ^
      (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
    ).toString(16)
  );
}
import axios from "axios";
export default {
  props: {
    baseUrl: {
      type: String,
      default: "http://localhost:3000",
    },
    //posts အပြင် တခြား endpoint ေတွအတွက်ပါ reusable ဖြစ် ေအာင် props ေတာင်းပါ့မယ်
    endpoint: {
      type: String,
      required: true,
    },
    // specific post တစ်ခုတည်းကို fetch ချင်ရင်ရ ေအာင် postIDေတာင်းပါမယ်
    postID: {
      type: [Number, String],
    },
  },
  data() {
    return {
      data: null,
      loading: false,
    };
  },
  created() {
    //component create လုပ်တဲ့အချိန်မှာ postID propsမ ေပးထားရင် postအကုန် fetchပါမယ်
    //ေပးထားရင် ေတာ့ အက် ေကာင်ကိုပဲfetchလုပ်မယ်
    if (this.postID) {
      this.read();
    } else {
      this.readAll();
    }
  },
  methods: {
    async crud(type, ...params) {
      this.loading = true;
      await axios[type](...params)
        .then((response) => {
          this.data = response.data;
          //success ဖြစ်ရင် တစ်ခုခုလုပ်လို့ရ ေအာင် parent ကို succes event တစ်ခု emitေပးလိုက်ပါမယ်
          this.$emit(`success`, this.data);
          this.loading = false;
        })
        .catch((error) => {
          //error ဖြစ်ရင် တစ်ခုခုလုပ်လို့ရ ေအာင် parent ကို error event တစ်ခု emitေပးလိုက်ပါမယ်
          this.$emit("error", error);
          this.loading = false;
        });
    },
    readAll() {
      return this.crud("get", `${this.baseUrl}/${this.endpoint}`);
    },
    read() {
      return this.crud(
        "get",
        `${this.baseUrl}/${this.endpoint}/${this.postID}`
      );
    },
    create(data) {
      data.id = uniqueId();
      return this.crud("post", `${this.baseUrl}/${this.endpoint}`, data);
    },
    destroy(id) {
      return this.crud("delete", `${this.baseUrl}/${this.endpoint}/${id}`);
    },
    update(id, data) {
      return this.crud("patch", `${this.baseUrl}/${this.endpoint}/${id}`, data);
    },
  },
  //render function က ေန vnode ေတွparent ကိုေပးရမယ့်အစား parent ရဲ့ v-slotထဲကို slot scopeအ ေနနဲ့ dataေတွmethod ေတွ pass လုပ် ေပးလိုက်ပါ့မယ်
  //ပုံမှန်ဆို render function က h() functionက ေန vnode create လုပ်ပြီး vnode ေတွ return ပြန် ေပးေလ့ ရိုပါတယ်
  render() {
    return this.$scopedSlots.default({
      loading: this.loading,
      data: this.data,
      create: this.create,
      read: this.read,
      readAll: this.readAll,
      destroy: this.destroy,
      update: this.update,
    });
  },
};
</script>
```
