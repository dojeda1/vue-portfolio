<template>
<h5>You encountered a Slime...</h5>
    <p class="dom-green-2-text">
        <i class="material-icons left">adb</i>
        Slime
        <span class="white-text"> | </span>
        <span class="">HP: 28/28</span>
        <span class="white-text"> | </span>
        <span class="">MP: 8/8</span>
    </p>
    <p class="dom-blue-text">
        <i class="material-icons left">person</i>
        Tom
        <span class="white-text"> | </span>
        <span class="dom-blue-text">HP: 28/28</span>
        <span class="white-text"> | </span>
        <span class="dom-blue-text">MP: 8/8</span>
    </p>
    <p class="dom-blue-text">
        <span class="grey-text">XP: 0/50</span>
        <span class="white-text"> | </span>
        <span class="red-text">$10</span>
    </p>
    <div class="message-box">
        <p>Message 1</p>
        <p>Message 2</p>
    </div>
    <p>What next?</p>
    <p>
        <button>Attack</button>
    </p>
    <p>
        <button>Use Item</button>
    </p>
    <p>
        <button><i className="material-icons left">arrow_back</i>Run</button>
    </p>
</template>

<script>
export default {
    name: 'Battle',
    data() {
        return {
            x: true,
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .message-box {
        border: 2px solid gray;
        padding: 10px;
        min-height: 100px;
    }
</style>
