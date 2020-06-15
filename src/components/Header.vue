<template>
    <div>
        <b-nav tabs>
            <b-nav-item> # </b-nav-item>
            <b-nav-item>Counter : {{correctAnswers}}/{{totalAnswers}}</b-nav-item>
        </b-nav>
    </div>
</template>
<script>
    export default {
        props: [
            'correctAnswers',
            'totalAnswers'
        ]
    }
</script>
   