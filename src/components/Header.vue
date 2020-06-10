<template>
    <div>
            <b-nav tabs>
                <b-nav-item disabled><b>Quiz App</b></b-nav-item>
                <b-nav-item disabled>Counter:  {{ number_correct }}/{{ number_total }}</b-nav-item>
            </b-nav>
    </div>
</template>

<script>
    export default {
        name: "Header",
        props: [
            'number_correct',
            'number_total'
        ]
    }
</script>

<style scoped>

</style>
