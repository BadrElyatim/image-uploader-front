<template>
    <div class="px-7 py-9">
        <h2 class="text-center text-lg font-medium text-[#4F4F4F] tracking-tight mb-5">Upload your image</h2>

        <p class="text-center text-[#828282] mb-7 font-medium text-xs tracking-tight">File should be Jpeg, Png...</p>

        <div class="px-24 py-12 border-2 border-[#97BEF4] border-dashed rounded-xl" @dragover="dragover"
            @dragleave="dragleave" @drop="drop">
            <img class="m-auto mb-10" src="../assets/images/uploader.svg" alt="">

            <p class="text-center font-medium text-xs tracking-tight text-[#BDBDBD]">Drag & Drop your image here</p>

        </div>

        <p class="text-center text-sm font-medium text-[#BDBDBD] my-8">Or</p>

        <input type="file" accept="image/*" id="file" class="hidden" ref="file" @change="onChange">
        <label for="file"
            class="px-3 py-2 m-auto block max-w-max cursor-pointer bg-[#5e8bc5] rounded-lg text-white font-medium">Choose
            a file</label>

        <div v-if="isUploading" id="loading-screen" class="w-full h-full fixed block top-0 left-0 bg-white opacity-75 z-50">
            <span class="text-green-500 opacity-75 top-1/2 -translate-y-1/2 my-0 mx-auto block relative w-max">
                <svg aria-hidden="true" class="mr-2 w-16 h-16 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
                    viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path
                        d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                        fill="currentColor" />
                    <path
                        d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                        fill="currentFill" />
                </svg>
                <span class="block mt-4 text-blue-600">Loading...</span>
            </span>
        </div>

    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'ImageUploader',
    data() {
        return {
            file: null,
            uploadPercentage: 0,
            isUploading: false,
        }
    },
    methods: {
        dragover(event) {
            event.preventDefault();

            if (!event.currentTarget.classList.contains('bg-[#cedef3]')) {
                event.currentTarget.classList.add('bg-[#cedef3]')
            }
        },
        dragleave(event) {
            event.currentTarget.classList.remove('bg-[#cedef3]')
        },
        drop(event) {
            event.preventDefault()



            this.$refs.file.files = event.dataTransfer.files
            this.onChange()

            // remove background
            event.currentTarget.classList.remove('bg-[#cedef3]')
        },
        onChange() {
            this.isUploading = true

            this.file = this.$refs.file.files[0]

            let formData = new FormData()

            formData.append('file', this.file)


            axios.post('http://localhost:3000/api',
                formData,
                {
                    headers: {
                        'Content-Type': 'multipart/form-data'
                    },
                    onUploadProgress: function (progressEvent) {
                        this.uploadPercentage = parseInt(Math.round((progressEvent.loaded / progressEvent.total) * 100));

                    }.bind(this)
                }
            ).then((res) => {
                console.log('SUCCESS!!');
                this.$router.push('/image/' + res.data)
                
            })
            .catch(() => {
                console.log('FAILURE!!');
                this.isUploading = false
            });
        }
    }
}
</script>