<template>
  <div class="main">
        <div class="nav"><h1 class="issue_title">Title</h1></div>
        <div class="body">
            <div class="speakers">

            </div>
            <div class="conversation">

            </div>
        </div> 
  </div>
</template>

<script>
export default {
  name: 'Home'
}
</script>

<style>
body {
    margin: 0;
}
.main {
    width: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
}

.nav {
    border: 1px solid black;
    color: white;
    width: 100%;
    height: 50px;
    background-color: #393b46;
    display: flex;
    align-items: center;
    justify-content: start;
    padding-left: 1rem;
}

.issue_title {
    font-style: normal;
}

.body {
    width: 100%;
    border: 1px solid black;
    display: flex;
    align-items: center;
    justify-content: center;
}

.speakers {
    border: 1px solid black;
    background-color: white;
    height: 500px;
    width: 40%;
}

.conversation {
    height: 500px;
    border: 1px solid black;
    background-color: lightgrey;
    width: 60%;
}
</style>
