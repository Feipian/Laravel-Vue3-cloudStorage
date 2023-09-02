<template>
    <modal :show="modelValue" @show="onShow" max-width="sm">
        <div class="p-6">
            <h2 class="text-lg font-medium text-gray-900">
                Create New Folder
            </h2>
            <div class="mt-6">
                <InputLabel for="folderName" value="Folder Name" class="sr-only"></InputLabel>
                <TextInput type="text" 
                            ref="folderNameInput"
                            id="folderName"
                            v-model="form.name"
                            class="mt-1 block w-full"
                            :class="form.errors.name ? 'border-red-500 focus:border-red-500 focus:ring-red-500' : ''"
                            placeholder="Folder Name"
                            @keyup.enter="createFolder"
                            >      
                </TextInput>
                <InputError :message="form.errors.name" class="mt-2"></InputError>
            </div>
            <div class="mt-6 flex justify-end">
                <SecondaryButton @click="closeModal">Cancel</SecondaryButton>
                <PrimaryButton class="ml-3" 
                                :class="{'opacity-25' : form.processing}"
                                 @click="createFolder" :disable="form.processing">
                    Submit
                </PrimaryButton>
            </div>
        </div>
    </modal>
</template>

<script setup>
import Modal from '@/Components/Modal.vue'
import InputLabel from '@/Components/InputLabel.vue';
import InputError from '../InputError.vue';
import TextInput from '../TextInput.vue';
import { useForm } from '@inertiajs/vue3';
import SecondaryButton from '../SecondaryButton.vue';
import PrimaryButton from '../PrimaryButton.vue';
import { ref } from 'vue';
import { nextTick } from 'vue';

// Uses
const form = useForm({
    name: ''
})

// Refs
const folderNameInput = ref(null)



// Props & Emit
const {modelValue} = defineProps({
    modelValue: Boolean
})

const emit = defineEmits(['update:modelValue'])

// Methods
function onShow(){
    nextTick( () => folderNameInput.value.focus())
  
}

function createFolder(){
    form.post(route('folder.create'),{
        preserveScroll: true,
        onSuccess: () => {
            closeModal()
            form.reset()
        },
        onError: () => {
            folderNameInput.value.focus()
        }
    })
}

function closeModal(){
    emit('update:modelValue')
    form.clearErrors();
    form.reset()
}
</script>

<style lang="scss" scoped>

</style>