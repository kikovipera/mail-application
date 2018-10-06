<template>

    <aside class="sm-side">
        <div class="user-head">
            <img src="src/assets/images/profile.jpg">

            <div class="user-name">
                <h5>Emmanuel Ashaolu</h5>
                <span class="email-address">info@codingexplained.com</span>
            </div>
        </div>

        <div class="compose-wrapper">
            <app-compose></app-compose>
        </div>

        <ul class="inbox-nav">
            <li :class="{active : activeView == 'app-inbox'}">
                <a href="#" @click.prevent="navigate('app-inbox', 'Inbox')">
                    <i class="fa fa-inbox"></i>Inbox <span class="badge badge-danger float-right">{{ unreadMessages.length }}</span>
                </a>
            </li>

            <li :class="{active : activeView == 'app-sent'}">
                <a href="#" @click.prevent="navigate('app-sent', 'Sent')">
                    <i class="fa fa-envelope"></i>Sent <span class="badge badge-primary float-right">{{ sentMessages.length }}</span>
                </a>
            </li>

            <li :class="{active : activeView == 'app-important'}">
                <a href="#" @click.prevent="navigate('app-important', 'Important')">
                    <i class="fa fa-bookmark"></i>Important <span class="badge badge-warning float-right">{{ importantMessages.length }}</span>
                </a>
            </li>

            <li :class="{active : activeView == 'app-trash'}">
                <a href="#" @click.prevent="navigate('app-trash', 'Trash')">
                    <i class="fa fa-trash"></i>Trash <span class="badge badge-primary float-right">{{ trashedMessages.length }}</span>
                </a>
            </li>

        </ul>

    </aside>

</template>

<script>
    import {eventBus} from './main.js';
    import Compose from './Compose.vue';
    
    export default{
        props:{
            messages:{
                type: Array,
                required: true
            }
        },
        created: function () {
            var vm = this;
            eventBus.$on('changeView', function (data) {
                vm.activeView = data.tag;
            });
        },
        data: function () {
            return {
                activeView: 'app-inbox'
            }
        },
        methods:{
            navigate: function (newView, title) {
                eventBus.$emit('changeView',{
                   tag: newView,
                    title: title
                });
            }
        },
        computed:{
            unreadMessages: function () {
                return this.messages.filter(function (message) {
                    return (message.type == 'incoming' && !message.isRead && !message.isDeleted);
                });
            },
            sentMessages: function () {
                return this.messages.filter(function (message) {
                    return (message.type == 'outgoing' && !message.isDeleted);
                });
            },
            importantMessages: function () {
                return this.messages.filter(function (message) {
                    return (message.type == 'incoming' && message.isImportant && !message.isDeleted);
                });
            },
            trashedMessages: function () {
                return this.messages.filter(function (message) {
                    return message.isDeleted;
                });
            },
        },
        components: {
            appCompose: Compose
        }
    }
</script>