<template>
    <div class="absolute font-medium -bottom-64 lg:-bottom-20 flex-col flex lg:flex-row bg-white gap-y-4 gap-x-12 px-8 pt-8 pb-12 rounded">
        <div class="flex flex-col text-sm mb-2 uppercase">
            <h3>IP Address</h3>
            <span>{{IPinfo.address}}</span>
        </div>
        <div class="flex flex-col text-sm mb-2 uppercase">
            <h3>Location</h3>
            <span>{{IPinfo.location}}</span>
        </div>
        <div class="flex flex-col text-sm mb-2 uppercase">
            <h3>Timezone</h3>
            <span>{{IPinfo.timezone}}</span>
        </div>
        <div class="flex flex-col text-sm mb-2 uppercase">
            <h3>ISP</h3>
            <span>{{IPinfo.isp}}</span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'IP-info',
    components:{},
    props: ['IPinfo']
}
</script>