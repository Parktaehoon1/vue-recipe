<template>
  <div class="wrap">
    <header class="header">
      <div class="search-box">
        <i class="fa-solid fa-magnifying-glass appear-btn"></i>
        <div class="show-btn">
          <input type="text" id="search-input" />
          <button id="search-btn">
            <i class="fa-solid fa-magnifying-glass"></i>
          </button>
        </div>
      </div>
      <h1>Food Recipe</h1>
    </header>
    <nav class="menu-list">
      <button>Bulgogi</button>
      <button>kimchi</button>
      <button>chicken</button>
      <button>beef</button>
      <button>salad</button>
      <button>fish</button>
      <button>pizza</button>
    </nav>
    <section id="food-board"></section>
  </div>
</template>

<script>
import { onMounted } from "vue";
export default {
  setup() {
    onMounted(() => {
      let menus = document.querySelectorAll(".menu-list button");
      menus.forEach((item) =>
        item.addEventListener("click", (event) => getFoodByTopics(event))
      );

      const getLatestfood = async () => {
        let url = new URL(
          `https://api.edamam.com/api/recipes/v2?type=public&q=bulgogi&app_id=d232b022&app_key=18c85995b5bccd69f64452bf8de8924a`
        );

        // let header = new Headers({
        //   q: "bulgogi",
        //   "api-id": "d232b022",
        //   "api-key": "18c85995b5bccd69f64452bf8de8924a",
        // });
        // let response = await fetch(url, { headers: header });
        let response = await fetch(url);
        let data = await response.json();
        let recipe = data.hits;
        console.log(recipe);
        // digest = recipe[0].recipe.digest;
        // for (let i = 0; i < recipe.length; i++) {
        //   // console.log("label", recipe[i].recipe.digest[0].label);
        //   for (let j = 0; j < digest.length; j++) {
        //     // console.log("label", recipe[i].recipe.digest[j].label);
        //     // index = recipe[i].recipe.digest[j].label;
        //   }
        // }
        render(recipe);
      };

      const getFoodByTopics = async (event) => {
        let topic = event.target.textContent;
        let url = new URL(
          `https://api.edamam.com/api/recipes/v2?type=public&q=${topic}&app_id=d232b022&app_key=18c85995b5bccd69f64452bf8de8924a`
        );
        let response = await fetch(url);
        let data = await response.json();
        let recipe = data.hits;
        console.log(recipe);
        render(recipe);
      };

      const getFoodByKeyword = async () => {
        // 검색 키워드 읽어오기
        // url에 검색 키워드 붙이기
        // header 준비
        // url 부르기
        // 데이터 가져오기
        // 데이터 보여주기

        let keyword = document.getElementById("search-input").value;
        console.log(keyword);
        let url = new URL(
          `https://api.edamam.com/api/recipes/v2?type=public&q=${keyword}&app_id=d232b022&app_key=18c85995b5bccd69f64452bf8de8924a`
        );
        let response = await fetch(url);
        let data = await response.json();
        let recipe = data.hits;
        console.log(recipe);
        render(recipe);
      };

      const render = (recipe) => {
        let foodHTML = "";
        let foodArr = [];
        foodArr = recipe.map((item) => {
          let tag = `<div class="food-list">
        <div class="food-img">
            <img class="food-img-size" src="${item.recipe.image}" alt="">
        </div>
        <div class="food-desc">
            <h2>${item.recipe.label}</h2>
            <p>
                식사 타입 : ${item.recipe.dishType}
            </p>
            <p>
                추천 시간 : ${item.recipe.mealType}
            </p>
            <div>
                재료 : ${item.recipe.ingredientLines}
            </div>

            <div>
                영양성분 :`;

          // 상세정보
          // let disgestTotal = item.recipe.digest.length;
          let disgestHtml = "";
          // for (let i = 0; i < disgestTotal; i++) {
          //   let obj = item.recipe.digest[i];
          //   disgestHtml += `${obj.label}: ${Math.floor(obj.total)} ${obj.unit}`;
          // }
          // tag += disgestHtml;

          item.recipe.digest.map((digestItem) => {
            disgestHtml += `${digestItem.label}: ${Math.floor(
              digestItem.total
            )} ${digestItem.unit}`;
          });
          tag += disgestHtml;
          // 마무리 코드
          tag += `</div>
            <div>
                총 영양성분 : ${Math.floor(item.recipe.totalWeight)} cal
            </div>
            <a href="${
              item.recipe.url
            }" target='_blank' class="recipe-btn">레시피 보러가기</a>
        </div>
    </div>`;
          return tag;
        });

        foodHTML = foodArr.join("");
        document.getElementById("food-board").innerHTML = foodHTML;
      };

      getLatestfood();

      let searchBtn = document.getElementById("search-btn");
      console.log("버튼", searchBtn);
      searchBtn.addEventListener("click", getFoodByKeyword);

      let appearBtn = document.querySelector(".appear-btn");
      console.log("appearbtn", appearBtn);
      let showBtn = document.querySelector(".show-btn");
      console.log("showbtn", "show-btn");

      let searchInputShow = () => {
        console.log("클릭");
        showBtn.style.opacity = "1";
        appearBtn.style.display = "none";
      };
      appearBtn.addEventListener("click", searchInputShow);
    });

    return {};
  },
};
</script>

<style>
@charset 'utf-8';
@import url("https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css");
/* font-family: 'Roboto', sans-serif; */
@import url("https://fonts.googleapis.com/css2?family=Jua&family=Roboto:wght@100;300;400;500;700;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  /* outline-style: none; */
}

ul,
li {
  list-style: none;
}

img {
  vertical-align: middle;
  border: 0;
}

a {
  color: #777;
  text-decoration: none;
}

html {
  font-size: 5px;
}

body {
  font-family: "Pretendard", "Malgun Gothic", "Dotum", "AppleGothic", sans-serif;
  font-size: 14px;
  font-weight: 400;
  line-height: 1.5;
  letter-spacing: -0.45px;
  overflow-x: hidden;
}

.wrap {
  position: relative;
  display: block;
  width: 1200px;
  margin: 0 auto;
}

.header {
  position: relative;
  display: block;
  text-align: center;
  margin: 20px;
}

.search-box {
  position: absolute;
  left: 0;
  top: 10px;
  display: block;
}
.appear-btn {
  cursor: pointer;
}
.show-btn {
  transition: all 0.3s;
  opacity: 0;
}
#search-btn {
  border: 0;
  background-color: transparent;
}
#search-btn > i {
  font-size: 16px;
}

.header > h1 {
  text-transform: uppercase;
}

.menu-list {
  position: relative;
  display: flex;
  justify-content: space-evenly;
  border: 1px solid #333;
}
.menu-list button {
  position: relative;
  border: 0;
  display: inline-block;
  margin: 0 10px;
  padding: 5px;
  font-size: 0.8vw;
  background: transparent;
  transition: all 0.3s;
  cursor: pointer;
  text-transform: uppercase;
}

.menu-list button:hover {
  color: red;
}

.food-list {
  position: relative;
  display: grid;
  grid-template-columns: auto 1fr;
  margin: 20px 0;
  border: 1px solid black;
}
.food-img {
  position: relative;
  display: block;
  width: 100%;
}
.food-img-size {
  max-width: 100%;
  max-height: 250px;
}
.food-desc {
  position: relative;
  display: block;
  height: 60%;
  padding: 10px;
}

.recipe-btn {
  position: relative;
  display: inline-block;
  font-size: 15px;
  color: #333;
  padding: 2px 5px;
  transition: all 0.5s;
}
.recipe-btn:hover {
  color: black;
  font-weight: 700;
}

.recipe-btn::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 3px;
  display: block;
  width: 0%;
  height: 2px;
  background-color: #333;
  transition: all 0.3s;
}
</style>
