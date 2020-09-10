<template>
  <div class="search-input-container relative w-11/12 md:w-2/6 mx-auto">
    <div>
      <label for="phone">Phone number</label>
      <div>
        <div
          @click="searchBoxVisiable = !searchBoxVisiable"
          class="text-gray-600 text-sm flex text-lg cursor-pointer pl-4 p-2 relative w-1/4 outline-none border float-left"
        >
          {{countryCode}}
          <svg width="6" height="3" viewBox="0 0 6 3" class="solid-arrow">
            <path
              d="M3.141 2.859L5.66.34A.2.2 0 0 0 5.517 0H.483A.2.2 0 0 0 .34.341L2.86 2.86a.2.2 0 0 0 .282 0z"
              fill="currentColor"
              fill-rule="evenodd"
            />
          </svg>
        </div>
        <input
          class="w-9/12 p-2 outline-none border border-l-0 text-lg"
          id="phone"
          v-model="phoneNumber"
        />
      </div>
    </div>
    <div v-if="searchBoxVisiable" class="search-country-code absolute w-full mt-1 rounded bg-white">
      <div class="search-input">
        <div class="search-el w-full">
          <input type="text" v-model="searchValue" placeholder="Search" />
          <span class="el-input-prefix">
            <i class="search-icon"></i>
          </span>
        </div>
      </div>
      <div class="list-scroll">
        <div v-if="searchedCountryCodes.length > 0" class="data-list">
          <div
            class="data-item"
            v-for="(item, index) in searchedCountryCodes"
            :key="item.name + item.dial_code + index"
            @click="onClickDataListItem(index)"
          >
            <span class="item-code">{{ item.code }}</span>
            <span class="country-name">[{{ item.name }}]</span>
            <span class="dial-code">{{ item.dial_code }}</span>
          </div>
        </div>
        <div v-else class="no-data">
          <svg width="42" height="42" viewBox="0 0 42 42" class="data-empty-icon">
            <path
              d="M11.686 2.397c1.844.155 3.306 1.698 3.393 3.61l.004.18V8.85a.375.375 0 0 0 .297.372l.073.008h22.08v27.537c0 1.979-1.681 3.582-3.754 3.582H4.5c2.072 0 3.753-1.603 3.753-3.582V5.966c0-1.862 1.489-3.392 3.392-3.566l.042-.003zM7.63 31.744v4.712c0 1.835-1.445 3.347-3.307 3.557C2.45 39.8 1 38.267 1 36.408v-4.664h6.63zm23.805-3.605H16.547c-.514 0-.931.372-.931.831 0 .428.363.78.83.826l.1.005h14.889c.514 0 .93-.372.93-.83 0-.46-.416-.832-.93-.832zm0-5.815H16.547c-.514 0-.931.372-.931.83 0 .429.363.781.83.826l.1.005h14.889c.514 0 .93-.372.93-.83 0-.46-.416-.831-.93-.831zm-7.444-5.816h-7.444c-.514 0-.931.372-.931.83 0 .429.363.781.83.827l.1.004h7.445c.514 0 .93-.372.93-.83 0-.46-.416-.831-.93-.831zM37.296 1.65C39.342 1.651 41 3.354 41 5.454v2.663c0 .21-.166.38-.37.38H16.184a.375.375 0 0 1-.37-.38V5.454c-.166-2.162-2.023-3.803-4.069-3.803h25.55z"
              fill="currentColor"
              fill-rule="nonzero"
            />
          </svg>
          <p class="data-empty-text">No record</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      searchBoxVisiable: false,
      phoneNumber: "",
      countryCode: "+82",
      searchValue: "",
      countryCodes: [],
    };
  },
  computed: {
    searchedCountryCodes() {
      const { searchValue } = this;

      if (!searchValue) {
        return this.countryCodes;
      }

      return this.countryCodes.filter((item) => {
        const { code = "", name = "", dial_code = "" } = item;
        if (
          code.toLowerCase().includes(searchValue.toLowerCase()) ||
          name.toLowerCase().includes(searchValue.toLowerCase()) ||
          dial_code.includes(searchValue)
        ) {
          return true;
        }
        return false;
      });
    },
  },
  mounted() {
    this.getCountryCodes();
  },
  methods: {
    getCountryCodes() {
      this.countryCodes = require("@/assets/data/CountryCodes.json").filter(
        (item) => {
          const { code, name, dial_code } = item;
          if (!code || !name || !dial_code) {
            return false;
          }
          return true;
        }
      );
    },
    onClickDataListItem(itemIndex) {
      const selectedItem = this.searchedCountryCodes[itemIndex];
      const { dial_code } = selectedItem;
      this.countryCode = dial_code;
      this.searchBoxVisiable = false;
    },
  },
};
</script>
<style lang="scss" scoped>
.search-input-container {
  .search-country-code {
    box-shadow: 0 10px 20px 0 rgba(0, 0, 0, 0.12);
    padding-bottom: 8px;
    max-width: 400px;

    .search-input {
      padding: 10px 10px 8px;

      .search-el {
        input {
          height: 40px;
          line-height: 40px;
          padding-left: 40px;
          border-radius: 4px;
          width: 100%;
          border: 1px solid #dcdfe6;
        }

        span.el-input-prefix {
          left: 4px;
          display: flex;
          align-items: center;
          padding-right: 5px;

          i.search-icon {
            background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAMAAACdt4HsAAAAWlBMVEUAAADMzMzPz8/Nzc3Ozs7Nzc3Nzc3MzMzNzc3Nzc3MzMzNzc3Q0NDX19fOzs7Ozs7Ozs7MzMzR0dHMzMzOzs7Q0NDNzc3Nzc3R0dHNzc3Nzc3Ozs7Nzc3MzMy24sYvAAAAHXRSTlMA2TOfD+7k9/PHvlAmC4aBVOwh+z8aa7ErzHpJQmpsGNkAAAD6SURBVFjD7ZVJjoMwEEXL2AYzGAKEJD28+1+zRUvdEgIpJrXJwm/nL9ly/Zokk8lknlIMjQnBNEMhr3Ax/GMucpapZkM9ySmuI+Bt28XYtdYD4/XU/Rmq3v0dXV/BfOKFaYSy2PhZwpgeRQ3lspWWEupk/6EqdjmtIDUXBvq92oNJrB/wbi87D2kVNYA90i0MkkID7ZHeQpNqQXekd6kmBIhHeoSgesBBUIegNlGdRnUhaUtZ3UzadlYOFO5RNdKAj0UzVC3rtzRj/bZ+q9Uslm8PfGpWW2cAG+V1XAPUiyj4tfIhClYrQyEKvjzcRcPDzDfR4SSTybwjP+O3Guhn7jmaAAAAAElFTkSuQmCC)
              50% no-repeat;
            position: absolute;
            top: 11px;
            display: inline-block;
            width: 36px;
            height: 36px;
            background-size: cover;
          }
        }
      }
    }

    .list-scroll {
      width: 100%;
      max-height: 216px;
      overflow: hidden;
      overflow-y: scroll;
      .data-list {
        .data-item {
          display: flex;
          padding: 0 20px;
          font-size: 13px;
          font-weight: 400;
          color: #999;
          line-height: 36px;
          cursor: pointer;

          &:hover {
            background: #f9fafc;
          }

          .item-code {
            color: #333;
            text-overflow: ellipsis;
          }
          .country-name {
            margin: 0 10px;
          }
          .dial-code {
            margin-left: auto;
          }
        }
      }

      .no-data {
        height: 216px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
      }
    }
  }
  .solid-arrow {
    position: absolute;
    right: 7px;
    top: 0;
    bottom: 0;
    margin: auto;
  }
}
</style>