<template>
    <div @click="$emit('update')">
           {{ title }}
    </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: "Mybox",
    props: {
        text: {
            title: {
            type: String,
            default: '+1'
        }
    }
    },

};


</script>

<style>

    button {
        background-color: #42b983;
        border: none;
        padding: 10px 20px;
        color: white;
        border-radius: 5px;
        cursor: pointer;
    }

    button:hover {
        background-color: #369870;
    }


</style>