<template>
  <div class="main">
        <div class="nav"><h1 class="issue_title">{{issue_title}} {{issue_number}}</h1></div>
        <div class="body">
            <div class="speakers">
                Author: <img :src="author.img" height="100px"> {{author.login}}
                <div class="speakers-list" v-for="speaker in speakers">
                    <img class="avatar" :src="speaker.avatar">
                    <p class="message">{{ speaker.login }}</p>
                </div>
            </div>
            <div class="conversation">
                <div class="conversation_header">Conversation with {{author.login}}</div>
                <div class="intervention" v-for="comment in comments">
                    <img class="avatar" :src="comment.user.avatar_url">
                    <p class="message">{{ comment.body }}</p>
                </div>
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
            issue_number: '',
            author: {},
            issue_title: 'default title',
            speakers: [],
            comments: {}
        }
    },
    created: function () {
        let tab = this.issue_url.split('/');
        this.issue_number = '#' + tab[6];
        let issue_for_title = 'https://api.github.com/repos/' + tab[3] + '/' + tab[4] + '/' + tab [5] + '/' + tab[6];
        axios.get(issue_for_title).then(response => {
            this.issue_title = response.data.title;
            this.author = {
                login: response.data.user.login,
                id: response.data.user.id,
                img: response.data.user.avatar_url
            };
        }).catch(error => {
            console.error('error on requesting API: ', error);
        });
        this.issue_url = issue_for_title + '/comments';
        axios.get(this.issue_url).then(response => {
            this.comments = response.data.reverse();
            this.uniq();
        }).catch(error => {
            console.error('error on requesting API: ', error);
        });

    },
    methods: {
        uniq: function() {
            let i = 0;
            let temp = [];
            this.comments.forEach(function(comment) {
                var isAlreadyExists = temp.find(function(speaker) {
                    return speaker === comment.user.login;
                });
                if (!isAlreadyExists) {
                    let speaker = {
                        login: comment.user.login,
                        id: comment.user.id,
                        avatar: comment.user.avatar_url
                    };
                    temp.push(speaker);
                }
            });
            this.speakers = temp;
        }
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
    color: white;
    width: 100%;
    height: 70px;
    background-color: #393b46;
    display: flex;
    align-items: center;
    justify-content: start;
}

.issue_title {
    font-style: normal;
}


.speakers {
    background-color: white;
    width: 40%;
    margin-top: 1rem;
}

.speakers-list {
    display: flex;
    align-items: flex-start;
    justify-content: flex-start;
}

.intervention {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: flex-start;
}

.conversation_header {
    margin-top: 1rem;
    align-self: flex-start;
}

@media screen and (min-width: 1000px){
    
    .body {
        width: 100%;
        display: flex;
        align-items: flex-start;
        justify-content: center;
    }

    .conversation {
        background-color: lightgrey;
        width: 60%;
    }

    .message {
        background-color: #d2f2ff;
        -webkit-box-shadow: 3px 3px 5px -1px rgba(184,184,184,1);
        -moz-box-shadow: 3px 3px 5px -1px rgba(184,184,184,1);
        box-shadow: 3px 3px 5px -1px rgba(184,184,184,1);
        width: 70%;
        border-radius: 2%;
        padding: 1rem;
    }

    .avatar {
        border-radius: 50%;
        margin: 1rem;
        /*height: 100px;*/
        width: 10%;
    }
}

@media screen and (max-width: 1000px) {
    .body {
        width: 100%;
        display: flex;
        align-items: flex-start;
        justify-content: center;
        flex-direction: column;
    }

    .conversation {
        background-color: lightgrey;
        width: 100%;
    }

    .message {
        background-color: #d2f2ff;
        -webkit-box-shadow: 3px 3px 5px -1px rgba(184,184,184,1);
        -moz-box-shadow: 3px 3px 5px -1px rgba(184,184,184,1);
        box-shadow: 3px 3px 5px -1px rgba(184,184,184,1);
        width: 65%;
        border-radius: 2%;
        padding: 1rem;
    }

    .avatar {
        border-radius: 50%;
        margin: 1rem;
        height: 50px;
    }
}

</style>
