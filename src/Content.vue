<template>
    <aside class="lg-side">
        <div class="inbox-head">
            <h3>{{ currentView.title }}</h3>
        </div>

        <keep-alive>
            <component :is="currentView.tag" :data="currentView.data"></component>
        </keep-alive>

    </aside>
</template>

<script>

    import Inbox from './Inbox.vue';
    import Sent from './Sent.vue';
    import Important from './Important.vue';
    import Trash from './Trash.vue';
    import ViewMessage from './ViewMessage.vue';
    import {eventBus} from './main.js';

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
             let temp = [{
                 tag: data.tag,
                 title: data.title,
                 data: data.data || {}
             }];
              vm.history = temp.concat(vm.history.splice(0));
          });
        },
        data: function () {
            return{

                history:[
                    {
                        tag: 'app-inbox',
                        title: 'Inbox',
                        data: {
                            messages: null
                        }
                    }
                ]

            }
        },
        computed: {
            currentView: function () {
                let current = this.history[0];
                current.data.messages = this.messages;
                return current;
            },
            previousView: function () {
                return typeof this.history[1] !== 'undefined' ? this.history[1] : null;
            }
        },
        components:{
            appInbox: Inbox,
            appSent: Sent,
            appImportant: Important,
            appTrash: Trash,
            appViewMessage: ViewMessage
        }
    }
</script>