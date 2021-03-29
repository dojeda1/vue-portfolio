<template>
<h5>Your adventure Begins...</h5>
    <p class="dom-blue-text">
        <i class="material-icons left">person</i>
        {{ $parent.player.name }}
        <span class="white-text"> | </span>
        <span class="dom-blue-text">HP: {{ $parent.player.hp }}/{{ $parent.player.maxHp }}</span>
        <span class="white-text"> | </span>
        <span class="dom-blue-text">MP: {{ $parent.player.mp }}/{{ $parent.player.maxMp }}</span>
        <span class="white-text"> | </span>
        <span class="grey-text">XP: {{ $parent.player.xp }}/{{ $parent.player.nextLevel }}</span>
        <span class="white-text"> | </span>
        <span class="red-text">{{ $parent.player.gold }}g</span>
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
