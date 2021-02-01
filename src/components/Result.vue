<template>
    <div>
        <p >Devise: {{deviseSelectedTmp}}</p>
        <p>{{result}}</p>
    </div>
</template>


<script>
    export default {
        props: ['result', 'deviseSelectedTmp']
    }
</script>
