<template>
  <nav class="topnav">
    <section class="categories">
      <a href="#" class="prev" @mousedown.prevent="moveLeft($event, 'positonCategories')"
       @mouseup.prevent="stopMoving($event, 'positonCategories')"
         @mouseleave.prevent="stopMoving($event, 'positonCategories')"
         @touchstart="moveLeft($event, 'positonCategories')"
         @touchend="stopMoving($event, 'positonCategories')"
         @touchcancel="stopMoving($event, 'positonCategories')">
        <icon name="prev" />
      </a>
      <div class="listBox">
        <ul>
          <li v-for="(category, index) in categories"
              :key="index"
              :class="{ active: category == 'Electronic' }">
            <a href="#">{{category}}</a>
          </li>
        </ul>
      </div>
      <a href="#" class="next" @mousedown.prevent="moveRight($event, 'positonCategories')"
       @mouseup.prevent="stopMoving($event, 'positonCategories')"
         @mouseleave.prevent="stopMoving($event, 'positonCategories')"
         @touchstart="moveRight($event, 'positonCategories')"
         @touchend="stopMoving($event, 'positonCategories')"
         @touchcancel="stopMoving($event, 'positonCategories')">
        <icon name="next" />
      </a>
    </section>
    <section class="tags">
      <a href="#" class="prev" @mousedown.prevent="moveLeft($event, 'positonTags')"
       @mouseup.prevent="stopMoving($event, 'positonTags')"
         @mouseleave.prevent="stopMoving($event, 'positonTags')"
         @touchstart="moveLeft($event, 'positonTags')"
         @touchend="stopMoving($event, 'positonTags')"
         @touchcancel="stopMoving($event, 'positonTags')">
        <icon name="prev" />
      </a>
      <div class="listBox">
      <ul>
        <li v-for="(tag, index) in tags.Electronic"
            :key="index"
            :class="{ active: tag == 'Disco' }"><a href="#">{{tag}}</a></li>
      </ul>
      </div>
      <a href="#" class="next" @mousedown.prevent="moveRight($event, 'positonTags')"
       @mouseup.prevent="stopMoving($event, 'positonTags')"
         @mouseleave.prevent="stopMoving($event, 'positonTags')"
         @touchstart="moveRight($event, 'positonTags')"
         @touchend="stopMoving($event, 'positonTags')"
         @touchcancel="stopMoving($event, 'positonTags')">
        <icon name="next" />
      </a>
    </section>
    <search-bar></search-bar>
  </nav>
</template>
<script>
import Icon from '@/components/Icon.vue';
import SearchBar from '@/components/SearchBar.vue';

export default {
  name: 'categories-menu',
  components: {
    Icon,
    SearchBar,
  },
  data() {
    return {
      interval: false,
      positonCategories: 0,
      positonTags: 0,
      categories: [
        'All Genres',
        'Blues',
        'Country',
        'Electronic',
        'Hip Hop',
        'Jazz',
        'Pop',
        'R&B and Soul',
        'Rock',
      ],
      tags: {
        Electronic: [
          'Ambient',
          'Breakbeat',
          'Disco',
          'Downtempo',
          'Drum and Bass',
          'Electro',
          'Hardcore',
          'House',
          'IDM',
          'Techno',
          'Trance',
        ],

      },
    };
  },
  methods: {
    moveLeft($e, menuLine) {
      if (!this.interval) {
        this.interval = setInterval((elem) => {
          const node = elem.nextSibling;
          const border = node.getElementsByTagName('ul')[0].offsetWidth - node.offsetWidth;
          if (Math.abs(this[menuLine]) < border) {
            this[menuLine] = this[menuLine] - 2;
            elem.nextSibling.getElementsByTagName('ul')[0].style.left = `${this[menuLine]}px`;
          }
        }, 20, $e.currentTarget);
      }
    },
    moveRight($e, menuLine) {
      if (!this.interval) {
        this.interval = setInterval((elem) => {
          if (this[menuLine] < 0) {
            this[menuLine] = this[menuLine] + 2;
            elem.previousSibling.getElementsByTagName('ul')[0].style.left = `${this[menuLine]}px`;
          }
        }, 20, $e.currentTarget);
      }
    },
    stopMoving() {
      clearInterval(this.interval);
      this.interval = false;
    },
  },
};
</script>
<style lang="scss" scoped>
  .topnav {
    font-size: 1.6rem;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    flex-grow: 1;
    position: sticky;
    top: 0;
    z-index: 1;

    > section {
      display: flex;
      justify-content: flex-start;
      align-items: center;
    }

    ul a {
      color: #737373;
      text-decoration: none;
      display: block;
      white-space: nowrap;
      text-transform: uppercase;
      padding: 2rem 1.7rem;
      border-bottom: 2px solid transparent;

      &:hover {
        color: #fff;
      }
    }

    .active {
      a {
        color: #fff;
        border-bottom: 2px solid #E41C69;
      }
    }

    .categories {
      background-color: #131314;
      overflow: hidden;
      position: relative;
      & > .listBox {
        position: relative;
        margin-left: 40px;
        margin-right: 40px;
        width: calc(100% - 94px);
        overflow: hidden;
        height: 61px;
        ul {
          position: absolute;
        }
      }
      & > .prev {
        position: absolute;
        left: 6px;
      }
      & > .next {
        position: absolute;
        right: 20px;
        top: 20px;
      }
    }

    .tags {
      background-color: #000;
      position: relative;
      & > .listBox {
        position: relative;
        margin-left: 40px;
        margin-right: 40px;
        width: calc(100% - 94px);
        overflow: hidden;
        height: 61px;
        ul {
          position: absolute;
        }
      }
      & > .prev {
        position: absolute;
        left: 6px;
      }
      & > .next {
        position: absolute;
        right: 20px;
        top: 20px;
      }
    }

    .categories, .tags {
      // padding-left: 2.5rem;

      ul {
        display: flex;
        justify-content: flex-start;
        align-items: flex-start;
        flex-grow: 0;

        li {
          display: block;
        }
      }
    }

    a.prev {
      transform: rotate(180deg);
    }
  }

  @media (min-width: 480px) {
    .categories, .tags {
      width: calc(100vw - 19rem);
    }
  }

  @media (min-width: 1200px) {
    a.prev, a.next {
      display: none !important;
    }
  }
</style>
