<template>
    <div :class="'board-item ' + (selected ? 'selected ' : '') + (closed ? ' closed' : '')">
        {{ icon ? icon: '' }}

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
}

.selected {
    background-image: url("../assets/tile_selected.png")
}

.closed {
    background-image: url("../assets/tile_walked.png")
}
</style>