<template>
    <div :class="
    'board-item ' 
    + (selected ? 'selected ' : '') 
    + (closed ? ' closed' : '')">
        <i v-if="icon" :class="'icon icon-' + icon"></i>

        <!-- здесь вместо icon iconid в v-if
        в борд.вуе тоже !!! -->
    </div>
</template>

<script>

export default {
    name: 'BoardItem',

    props: {
        icon: {
            type: Number,
            required: false,
            default: 0,
        },
        selected: {
            type: Boolean,
            required: false,
            default: false,
        },
        closed: {
            type: Boolean,
            required: false,
            default: false,
        },
    }
}

</script>

<style>
.board-item {
    width: 64px;
    height: 64px;
    background-image: url("../assets/tile.png");
    margin: 2px 2px;
    position: relative;
}

.selected {
    background-image: url("../assets/tile_selected.png")
}

.closed {
    background-image: url("../assets/tile_walked.png")
}

.icon {
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
}

.icon-1 {
    background: url('../assets/beast1.png');
}

.icon-3 {
    background: url('../assets/weapon1.png');
}

.icon-2 {
    background: url('../assets/beast5.png');
}

.icon-4 {
    background: url('../assets/weapon4.png');
}

.icon-7 {
    background: url('../assets/beast3.png');
}

.icon-9 {
    background: url('../assets/weapon3.png');
}

.icon-8 {
    background: url('../assets/beast4.png');
}

.icon-10 {
    background: url('../assets/weapon2.png');
}

.icon-13 {
    background: url('../assets/beast2.png');
}

.icon-15 {
    background: url('../assets/weapon5.png');
}




</style>