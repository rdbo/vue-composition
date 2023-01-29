<template>
    <div class="home">
        <p ref="p">Message: {{ message }}</p>
        <button @click="testRef">Change ref value</button>
        <br/>
        <input type="text" v-model="search">
        <p v-for="name in filteredNames">{{ name }}</p>
    </div>
</template>

<script>
import { computed, ref, watch, watchEffect } from "vue";

export default {
    name: 'HomeView',
    setup() {
        const p = ref(null);
        const search = ref('');
        const names = ref(['john', 'peter', 'tyler']);
        let count = 0;

        const testRef = () => {
            p.value.textContent = "Test";
        }

        const stopWatch = watch(search, () => {
            console.log("watch caught change in 'search': ", search.value);
            count++;
            if (count >= 5)
                stopWatch();
        });

        const stopWatchEffect = watchEffect(() => {
            console.log("watchEffect caught change in one of the following: ", p.value, search.value);
            count++;
            if (count >= 10)
                stopWatchEffect();
        });

        const filteredNames = computed(() => {
            return names.value.filter((name) => { return name.includes(search.value); })
        });

        return { message: "Using the Components API", p, testRef, names, search, filteredNames };
    }
}
</script>
