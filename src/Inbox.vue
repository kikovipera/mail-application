<template>
    <div class="inbox-body">
        <div class="mail-option">
            <div class="btn-group">
                <a href="#" class="btn" @click.prevent="refresh">
                    <i class="fa fa-refresh"></i>&nbsp;Refresh
                </a>
            </div>
        </div>

        <app-messages :messages="incomingMessages"></app-messages>
    </div>
</template>

<script>
    import Messages from './Messages.vue';
    import { eventBus } from './main.js';
    export default{
        props: {
            data:{
                type: Object,
                required: true
            }
        },
        methods: {
            refresh: function () {
                eventBus.$emit('refreshMessages');
            }
        },
        computed: {
            incomingMessages: function () {
                return this.data.messages.filter(function (message) {
                    return (message.type == 'incoming' && message.isDeleted === false);
                });
            }
        },
        components: {
            appMessages: Messages
        }
    }
</script>