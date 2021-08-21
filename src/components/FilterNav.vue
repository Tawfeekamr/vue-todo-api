<template>
    <nav class="filter-nav">
        <button @click="updateFilter('all')" :class="{active: current === 'all'}">View All</button>
        <button @click="updateFilter('completed')" :class="{active: current === 'completed'}">View Completed</button>
        <button @click="updateFilter('not-completed')" :class="{active: current === 'not-completed'}">View Not Completed</button>
    </nav>
</template>

<script>
    export default {
        name: "FilterNav",
        props: ['current'],
        methods: {
            updateFilter(by) {
                this.$emit('filterChanged', by);
            }
        }
    }
</script>

<style scoped>
    .filter-nav button {
        background: none;
        border: none;
        color: #bbb;
        outline: none;
        font-size: 12px;
        text-transform: uppercase;
        margin-right: 10px;
        letter-spacing: 1px;
        font-weight: bold;
        cursor: pointer;
    }
    .filter-nav button.active {
        color: #555;
    }
</style>