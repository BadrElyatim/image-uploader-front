<template>
    <div class="max-w-md">
        <img class="w-14 m-auto mb-2" src="../assets/images/check.svg" alt="">
        <h2 class="text-center font-medium mb-6 text-[#4F4F4F] tracking-tight text-xl">Uploaded Successfuly!</h2>

        <div class="">
            <img class=" rounded-xl w-full h-auto" :src="imageLink" alt="">
        </div>
        <div class="flex items-center mt-4 bg-[#F6F8FB] pl-3 pr-1 py-1 rounded-md border-2 border-[#E0E0E0]">
            <p class="text-ellipsis overflow-hidden whitespace-nowrap font-medium tracking-tight text-xs pr-6">
                {{this.imageLink}}</p>
            <button
                class="bg-[#2F80ED] rounded-md px-6 py-3 whitespace-nowrap text-white font-medium text-xs tracking-tight"
                @click="copy">Copy Link</button>
        </div>

        <input type="hidden" id="testing-code" :value="imageLink">
    </div>
</template>

<script>

export default {
    name: 'ImageUploaded',
    date() {
        return {
            imageLink: ''
        }
    },
    created() {
        this.imageLink = 'https://alert-crab-helmet.cyclic.app/api/image/' + this.$route.params.id
    },
    methods: {
        copy() {
            let testingCodeToCopy = document.querySelector('#testing-code')
            testingCodeToCopy.setAttribute('type', 'text') 
            testingCodeToCopy.select()

            try {
                document.execCommand('copy');

            } catch (err) {
                alert('Oops, unable to copy');
            }

            /* unselect the range */
            testingCodeToCopy.setAttribute('type', 'hidden')
        }
    }

}
</script>