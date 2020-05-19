<template>
    <div>
        <h1>Sendbird chat</h1>
        <form v-if="!user" class="flex flex-col space-y-5" @submit.prevent="login">
            <label>User ID: <input type="text" v-model="userId"></label>
            <button class="cursor-pointer">Login</button>
        </form>
        <form v-else @submit.prevent="sendMessage">
            <h2>Logged in as {{ user.userId }}</h2>
            <input type="text" v-model="message">
            <button>Send</button>
        </form>
    </div>
</template>

<script>
import SendBird from 'sendbird';

export default {
    name: 'Chat',

    data() {
        return {
            userId: null,
            user: null,
            message: null,
            channel: null,
        };
    },

    methods: {
        async login() {
            console.log(process.env.VUE_APP_APP_ID);
            const sb = new SendBird({ appId: process.env.VUE_APP_APP_ID });
            this.user = await sb.connect(this.userId);
            this.channel = await sb.OpenChannel.getChannel(process.env.VUE_APP_CHANNEL_URL);
            await this.channel.enter();
            this.channel.createPreviousMessageListQuery().load(console.log);
            const handler = new sb.ChannelHandler();
            handler.onMessageReceived = console.log;
            sb.addChannelHandler('console.log', handler);
        },
        sendMessage() {
            this.channel.sendUserMessage(this.message, console.log);
        }
    }
}
</script>