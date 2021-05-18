<template>
  <div id="app">
    <the-navbar></the-navbar>
    <the-header></the-header>
    <the-content></the-content>
  </div>
</template>

<script>
import TheContent from "./components/Layouts/TheContent.vue";
import TheHeader from "./components/Layouts/TheHeader.vue";
import TheNavbar from "./components/Layouts/TheNavbar.vue";

export default {
  name: "App",
  components: {
    TheNavbar,
    TheHeader,
    TheContent,
  },
};
</script>

<style>
@font-face {
  font-family: notosans-bold;
  src: url("./assets/fonts/notosans-bold.3ede5aff.woff2");
}
@font-face {
  font-family: notosans-italic;
  src: url("./assets/fonts/notosans-italic.1c1aaac9.woff2");
}
@font-face {
  font-family: notosans-regular;
  src: url("./assets/fonts/notosans-regular.2cb88a13.woff2");
}
@font-face {
  font-family: notosans-semibold;
  src: url("./assets/fonts/notosans-semibold.df3e6dc4.woff2");
}

body {
  margin: 0;
  font-family: notosans-regular;
  font-size: 13px;
}
a {
  text-decoration: none;
}
a:focus {
  outline: none;
}
input {
  border: 1px solid #babec5;
  border-radius: 4px;
  width: 100%;
  height: 32px;
  padding-left: 16px;
  padding-right: 32px;
  box-sizing: border-box;
}
input::placeholder {
  color: #babec5;
  font-family: notosans-italic;
  font-size: 12px;
}
input:hover {
  box-shadow: 0 0 5pt #d3d3d3;
}
input:focus {
  outline-color: #2ca01c;
}
li {
  list-style-type: none;
}
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}
::-webkit-scrollbar-thumb {
  background: #b8bcc3;
}

select {
  border-radius: 4px;
  width: 100%;
  height: 32px;
  padding-left: 10px;
  padding-right: 16px;
  box-sizing: border-box;
  border: 1px solid #babec5;
}
select:focus {
  border: 1px solid #2ca01c;
  outline-color: #2ca01c;
}
select option {
  width: 100%;
  height: 32px;
}

.flex {
  display: flex !important;
}
.pointer {
  cursor: pointer;
}
.mi {
  background: url("./assets/img/Sprites.64af8f61.svg") no-repeat;
}
.mi-24 {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px !important;
  height: 24px !important;
}
.mi-14 {
  width: 14px !important;
  height: 14px !important;
}
.mi-20 {
  width: 20px !important;
  height: 20px !important;
}
.mi-32 {
  width: 32px !important;
  height: 32px !important;
}


</style>
