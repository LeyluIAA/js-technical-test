<template>
  <div class="main">
        <div class="nav"><h1 class="issue_title">{{issue_title}} - {{issue_url}}</h1></div>
        <div class="body">
            <div class="speakers">

            </div>
            <div class="conversation">
                {{comments}}
            </div>
        </div> 
  </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'Home',
    data: function (){
        return {
            issue_url: prompt('please enter the issue URL'),
            issue_title: 'default title',
            comments: {}
        }
    },
    created: function () {
        let tab = this.issue_url.split('/');
        let issue_for_title = 'https://api.github.com/repos/' + tab[3] + '/' + tab[4] + '/' + tab [5] + '/' + tab[6];
        axios.get(issue_for_title).then(response => {
            this.issue_title = response.data.title;
            console.error('get this title', this.issue_title);
        }).catch(error => {
            console.error('error on requesting API: ', error);
        });
        this.issue_url = issue_for_title + '/comments';
        axios.get(this.issue_url).then(response => {
            this.comments = response.data;
            console.error('get this ', this.comments);
        }).catch(error => {
            console.error('error on requesting API: ', error);
        });

    },
    methods: {

    }
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
