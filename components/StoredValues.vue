<template>
  <div class="stored">
    <div class="stored_background"></div>
    <div class="stored_background2"></div>
    <h2 class="stored_title">Промежуточные итоги:</h2>
    <ul class="stored_list" v-if="stored.length > 0">
      <transition-group name="list">
        <li v-for="(st, idx) in stored" :key="idx" class="stored_item">
          {{ st.result }}
          <div>
            <span>{{ st.data }}</span>
            <span @click="$emit('removeStore', st)" class="stored_del">X</span>
          </div>
        </li>
      </transition-group>
    </ul>
    <div class="stored_else" v-else>
      Сохраненных результатов нет! Сохрани первый!
    </div>
  </div>
</template>

<script>
export default {
  props: {
    stored: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {};
  },
  methods: {},
};
</script>

<style lang="scss" scoped>
.stored {
  position: relative;
  flex: 0 1 45%;
  margin-left: 4rem;
  &_background,
  &_background2 {
    content: "";
    position: absolute;
    width: 18rem;
    height: 18rem;
    transform: rotate(25deg);
    background: rgb(32, 36, 36);
    box-shadow: 5px 10px 40px rgba(0, 0, 0, 0.568);
    z-index: 1;
  }
  &_background {
    bottom: 0;
    right: 0;
    transform: rotate(61deg);
    background: rgb(247, 91, 91);
  }
  &_background2 {
    transform: rotate(41deg);
    top: 20%;
  }
  &_title {
    font-size: 2.4rem;
    color: $main;
    font-weight: 700;
    margin-bottom: 2rem;
    position: relative;
    z-index: 100;
    text-shadow: 0px 1px 6px black;
  }

  &_list {
    position: relative;
    z-index: 5;
    backdrop-filter: blur(5px);
  }

  &_item {
    color: $main;
    font-size: 2.2rem;
    color: $yellow;
  }

  &_del {
    color: $yellow;
    cursor: pointer;
    font-weight: 700;
  }
  &_else {
    color: $yellow;
    font-size: 1.8rem;
    position: relative;
    z-index: 100;
    text-shadow: 0px 1px 6px black;
  }
}
li {
  margin-bottom: 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  padding: 0.4rem 0;
  border-bottom: 1px solid rgba($color: $yellow, $alpha: 0.3);
  &::before {
    content: "";
    position: absolute;
    top: 50%;
    left: -2rem;
    width: 1rem;
    height: 1rem;
    background: $yellow;
    transform: translateY(-50%);
  }
  span {
    font-size: 1.6rem;
    color: $main;
    margin-left: 1rem;
    text-shadow: 0px 0px 6px black;
    font-weight: 700;
  }
}
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active,
.list-leave-active {
  transition: all 0.4s ease;
}
.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translatex(130px);
  transition: all 0.4s ease;
}
</style>
