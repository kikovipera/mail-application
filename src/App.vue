<template>

    <div class="container">
        <div class="mail-box">
            <app-sidebar :messages="messages"></app-sidebar>
            <app-content :messages="messages"></app-content>
        </div>
    </div>

</template>

<script>

    import Sidebar from './Sidebar.vue';
    import Content from './Content.vue';
    import messages from './data/messages.js';
    import randomMessages from './data/random-messages.js';
    import { eventBus } from './main.js';

    export default {
        data: function () {
            return {
                messages: messages
            }
        },
        created: function () {
            var vm = this;
            eventBus.$on('refreshMessages', function () {
                let randomIndex = Math.floor(Math.random() * randomMessages.length);
                let temp = [randomMessages[randomIndex]];

                vm.messages = temp.concat(vm.messages.slice(0));
            });
            eventBus.$on('sentMessage', function (data) {
                let temp = [data.message];
                vm.messages = temp.concat(vm.messages.slice(0));
            });
        },
        components:{
            appSidebar: Sidebar,
            appContent: Content
        }

    }
</script>