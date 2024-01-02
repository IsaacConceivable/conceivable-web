<template>
    <v-container :fluid="true" >
        <v-row no-gutters>
            <v-col cols="12" md="3" class="pa-2">
                <v-label>
                    Rutines availables
                </v-label>
                <v-menu transition="scale-transition">
                    <template v-slot:activator="{ props }">
                        <v-btn
                            color="primary"
                            v-bind="props"
                            block
                            size="large"
                        >
                            {{ selectedRutine == null ? "Rutines" : selectedRutine.name }}
                        </v-btn>
                    </template>
                    <v-list>
                        <v-list-item
                        v-for="(item, index) in items"
                        :key="index"
                        :value="index"
                        @click="selectedRutine = item"
                        >
                        <v-list-item-title>{{ item.name }}</v-list-item-title>
                        </v-list-item>
                    </v-list>
                </v-menu>
                <v-card
                    class="mx-auto mt-2"
                    v-if="selectedRutine !== null"
                >
                    <v-list class="bg-orange-accent-2 text-grey-lighten-3">
                        <v-list-item
                            v-for="(rutine, index_r) in selectedRutine.rutines"
                            :key="`rutine_${index_r}`"
                            :value="rutine"
                            @click="rutinePrepared = rutine"
                        >
                            {{ selectedRutine.name.includes('Perl') ? `Rutine #${rutine}` : rutine }}
                        </v-list-item>
                    </v-list>
                </v-card>
                <v-btn class="mt-2" color="success" block size="large" v-if="rutinePrepared != null">
                    Start rutine <v-icon icon="mdi-play"/>
                </v-btn>
            </v-col>
            <v-col cols="12" md="6">
                <camera :resolution="{ width: 600, height: 480 }" autoplay></camera>
            </v-col>
            <v-col class="pa-2" cols="12" md="3">
                <v-btn block color="red" size="x-large">
                    Stop Rutine <v-icon icon="mdi-stop"/>
                </v-btn>
            </v-col>
        </v-row>

    </v-container>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from "vue";
import Camera from "simple-vue-camera";

export default defineComponent({
    components: {
        Camera,
    },
    setup(){
        const camera = ref<InstanceType<typeof Camera>>()
        const devices = camera.value?.devices()
        const selectedRutine = ref(null)
        const rutinePrepared = ref(null)
        const items = ref([
            {
                name: 'Perl 2',
                rutines: [1,2,3]
            }, 
            {
                name: 'Perl 3',
                rutines: [1,2,3]

            },
            {
                name: 'Perl 5',
                rutines: [1,2,3]
            },
            {
                name: 'Calibration Rutines',
                rutines: ['Home', 'Micromanipulator', 'Camera']
            }
        ])
        watch(selectedRutine, () => {
            rutinePrepared.value = null
        })
        return{
            devices,
            camera,
            selectedRutine,
            rutinePrepared,
            items,
        }
    }
});
</script>
