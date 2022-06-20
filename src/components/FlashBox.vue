<template>
    <div class="text-center alert alert-success flash">
        <span> {{ flashMsg}} </span>
    </div>
</template>

<script>
export default({
    name: 'FlashBox',
    props: {
        showflash: {
            type:Boolean
        },
        flashMsg: {
            type: String
        }
    }
})
</script>



