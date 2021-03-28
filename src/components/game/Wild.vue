<template>
<h5>Your adventure Begins...</h5>
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
    <p>Where to next?</p>
    <p>
        <button>Explore</button>
    </p>
    <p>
        <button>Go to Town</button>
    </p>
    <p>
        <button>Use Item</button>
    </p>
    <p>
        <button>To Forest<i className="material-icons">arrow_forward</i></button>
    </p>
</template>

<script>
export default {
    name: 'Wild',
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

</style>
