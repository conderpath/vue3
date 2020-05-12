<template>
  <div>
    <h1>vue3小试牛刀哦</h1>
    <div>
      <label for="">获取属性：</label>
      <span>{{ msg }}</span>
    </div>
    <div>
      <p>响应式数据：</p>
      <p>
        <span>{{ count }}</span>
        <input type="button" value="增加" @click="increase" />
        <span>{{ totals }}</span>
      </p>
      <p>
        <span>{{ name }}-</span>
        <span>{{ age }}-</span>
        <span>{{ address }}</span>
        <input type="button" value="修改" @click="modify" />
      </p>
      <p>
        <span>{{ info.a }}</span>
      </p>
      <p>
        <span>{{ info.b }}</span>
      </p>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, ref, computed, watch } from "@vue/composition-api";
export default {
  beforeCreate() {},
  setup() {
    // 创建响应式的data
    const state = reactive({
      name: "zhangsan",
      age: 10,
      address: "北京市"
    });
    const count = ref(0);
    const modify = () => {
      state.name = "李四";
    };
    const increase = () => {
      count.value++;
    };
    // 计算属性
    const totals = computed(() => {
      return count.value * 10;
    });
    // watch属性
    //1、ref对象时
    watch(count, () => {
      console.log("变化了");
    });
    //2、reactive对象时
    watch(
      () => state.name,
      (name, prevName) => {
        console.log(name, prevName);
      },
      {
        lazy: true //在watch创建的时候，不执行，在改变的时候才执行
      }
    );
    return {
      count,
      ...toRefs(state),
      info: ref({ a: 1, b: 2 }),
      modify,
      increase,
      totals
    };
  },
  created() {},
  props: {
    msg: String
  }
};
</script>

<style lang="less" scoped></style>
