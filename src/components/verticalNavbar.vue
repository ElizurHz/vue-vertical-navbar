<template>
  <div class="sub-nav">
    <ul :style="{ width: width + 'px'}" class="sub-nav-ul">
      <li 
        :class="{ hover: item.isHover, 'sub-nav-li': true }" 
        @mouseenter="liHover(index)" 
        @mouseleave="liLeave(index)" 
        v-for="(item, index) in nav" 
        :key="item.name"
      >
        <img class="logo nav-item" :src="item.isHover ? item.hoverUrl : item.url" />
        <div class="name nav-item">{{ item.name }}</div>
        <div class="description nav-item">{{ item.description }}</div>
      </li>
    </ul>
    <div :class="popOverHoverClass">
      <div class="category">
        <div class="category-title">{{ categoryTitle }}</div>
        <ul>
          <li 
            v-for="item in list"
            :key="item.name"
            class="category-name"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "verticalNavbar",
  props: {
    sideNavList: {
      type: Array,
      required: true
    },
    list: {
      type: Array,
      required: true
    },
    width: {
      required: true
    },
    categoryTitle: {
      type: String
    }
  },
  data() {
    return {
      nav: [],
      popoverIsHover: false,
      currentHoverIndex: -1
    }
  },
  mounted() {
    this.nav = this.props.sideNavList
  },
  computed: {
    popOverHoverClass() {
      return {
        'sub-nav-popover': true,
        hover: this.popoverIsHover === true
      }
    }
  },
  methods: {
    liHover(index) {
      this.popoverIsHover = true  
      this.currentHoverIndex = index
      this.nav[index].isHover = true
    },
    liLeave(index) {
      this.popoverIsHover = false  
      this.nav[index].isHover = false
    }
  }
}
</script>

<style scoped lang="scss">
@mixin vertical-center-relative {
  position: relative;
  top: 50%;
  transform: translateY(-50%);
}
.nav-item {
  display: inline-block;
  @include vertical-center-relative;
}
.sub-nav {
  position: absolute;
  .sub-nav-ul {
    z-index: 100;
    position: absolute;
    float: left;
    background-color: rgba(0, 0, 0, .6);
    height: 400px;
    margin: 0;
    .sub-nav-li {
      list-style: none;
      height: 80px;
      color: #fff;
      font-size: 14px;
      .logo {
        width: 40px;
        height: 40px;
        margin-left: 15px;
      }
      .name {
        margin-left: 10px;
      }
      .description {
        color: #888;
        font-size: 12px;
        margin-left: 10px;
      }
    }
    li.hover {
      background-color: #fff;
      color: #e34346;
    }
  }
  .sub-nav-popover {
    width: 960px;
    margin-left: 230px;
    height: 400px;
    background-color: #fff;
    z-index: 100;
    display: none;
  }
  .sub-nav-popover.hover {
    display: block;
  }
}
</style>
