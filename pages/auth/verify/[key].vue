<script setup>
import Cookies from "js-cookie";

import Axios from "axios";

const url = inject("url");

const kb = ref(false) //key broken


const vm = ref(false); // verified message. boolean to or not display verifed message

const { key } = useRoute().params;


const verify_account = async () => {
    const res = await Axios.post(`${url}auth/verify/`, { 'key': key });
    if (res.data) {

        if (res.data.status == "success") {
            document.querySelector(".head-text").innerHTML = "Account Verified";
            vm.value = true;
            const timeout = setTimeout(() => {
                useRouter().push("/user/");
                clearTimeout(timeout);
            }, 5000);

        } else {

            kb.value = true;
            const timeout = setTimeout(() => {
                useRouter().push("/auth/verify/");
                clearTimeout(timeout);
            }, 5000);

        }

    } else {
        useRouter().push("/auth/signin");
    }
}

onMounted(() => {
    verify_account();
})

</script>

<template>
    <div class="border border-transparent">
        <div class="w-full h-[calc(100vh-100px)] mt-[30px] relative grid content-center">
            <div class="max-w-sm !w-full mx-auto text-center py-5 px-2 shadow-md shadow-violet-300">
                <h2 class="text-2xl head-text">Verifying your account</h2>
                <p v-if="kb == true" class="mt-1 text-primary-hover">
                    This Link is broken or could not verify this account, redirecting.....
                </p>
                <p v-if="vm == true" class="mt-1 text-green-500">
                    please wait, redirecting.....
                </p>
                <p class="py-10">
                    <i class="fa fa-spinner text-2xl animate-spin"></i>
                </p>
            </div>
        </div>
    </div>
</template>

<style></style>
