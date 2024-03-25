<script>
export default {
  props: {
    styleType: {
      type: Number,
      default: 1,
    },
  },
};
</script>

<template>
   <button :class="['main__link', `type-${styleType}`]">
    <div class="main__link-text"><slot></slot></div>
    <div class="main__link-img" :class="`img-type-${styleType}`">
      <svg
        width="22"
        height="22"
        viewBox="0 0 22 22"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path d="M21 4.29346V20.952H4.37326M20.9618 21L1 1" stroke-width="2" />
      </svg>
    </div>
  </button>
</template>

<style lang="scss" scoped>
.main__link {
  display: flex;
  max-width: 380px;
  justify-content: space-between;
  cursor: pointer;
}

.main__link-text {
  align-self: center;
  font-family: "Raleway";
  font-weight: 600;
  font-size: 0.18rem;
  line-height: 22px;
  transition: all 0.3s ease;
  white-space: nowrap;
  padding-left: 24px;
  padding-right: 8px;
}

.main__link-img {
  align-self: center;
  width: 71px;
  height: 71px;
  padding: 38px 13px 13px 38px;
}

.main__link:hover .main__link-text {
  transform: scale(105%);
}

.main__link-img svg {
  transition: all 0.3s ease;
}

.main__link:hover .main__link-img svg {
  transform: scale(110%) translate(10%, 10%);
}

.type-1 {
  color: #0b3461;
  border: 1px solid #ffffff;
  background-color: rgba(255, 255, 255, 1);
}

.img-type-1 {
  border-left: 2px solid #0b3461;
  stroke: #0b3461;
}

.type-2 {
  color: #ffffff;
  border: 1px solid #ffffff;
  stroke: #ffffff;
  background-color: inherit;
}

.img-type-2 {
  border-left: 2px solid #ffffff;
  stroke: #ffffff;
}

@media (max-width: 768px) {
  .main__link {
    max-width: 200px;
  }
  .main__link .main__link-text {
    font-size: 0.15rem;
    padding-left: 23px;
  }
  .main__link .main__link-img {
    align-self: center;
    width: 40px;
    height: 50px;
    padding: 25px 10px 10px 20px;
  }
}
</style>
