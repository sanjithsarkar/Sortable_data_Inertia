
 <template>
    <div class="grid grid-cols-6 gap-4 p-4 bg-gray-100">
        <div class="col-span-3 md:col-span-1 flex flex-col gap-2  rounded">
            <h3 class="text-[16px]">Subjects</h3>

            <div>
                <draggable
                        ghostClass='ghost'

                        class="flex-col flex gap-2 min-h-[200px] bg-white rounded shadow p-4"
                        v-model="categoryData"
                        group="people"
                        item-key="id">
                    <template #item="{element}">
                        <div class="bg-white flex">
                            <p class="text-sm border-gray-100 cursor-move shadow p-2 bg-gray-50 rounded mt-2 w-full">
                                {{ element.name }}</p>
                        </div>
                    </template>
                </draggable>
            </div>
        </div>

        <div class="col-span-3 md:col-span-5 flex flex-col gap-2 rounded">
            <h3 class="text-[16px]">Subjects</h3>
            <div class="h-full bg-white rounded w-full overflow-scroll">
                <draggable
                        class="flex pb-4 lg:pb-0 h-full w-fit min-w-full"
                        handle=".handle"
                        ghostClass='ghost'
                        v-model="course"
                        @change="onCourseChange"
                        :component-data="getComponentData()"
                        group="people"
                        item-key="id">
                    <template #item="{element, index}">
                        <div class="h-fit w-[300px]">
                            <draggable
                                    :options="{swapThreshold: .01, direction:'vertical', ghostClass:'ghost'}"
                                    class="w-full p-4"
                                    v-model="course[index].data"
                                    group="people"
                                    @start="onChildDragStart"
                                    @end="onChildDragEnd"
                                    item-key="id">


                                <template #header>
                                    <div class="border-1 border-t border-l border-r px-2 py-2 bg-white flex gap-1 items-center  shadow">
                                        <span class="handle cursor-move">
                                                <svg class="w-4 h-4" xmlns="http://www.w3.org/2000/svg"
                                                     viewBox="0 0 15 15" fill="none"><path
                                                        d="M3 5.5a.5.5 0 11-1 0 .5.5 0 011 0zm5 0a.5.5 0 11-1 0 .5.5 0 011 0zm5 0a.5.5 0 11-1 0 .5.5 0 011 0zm-10 4a.5.5 0 11-1 0 .5.5 0 011 0zm5 0a.5.5 0 11-1 0 .5.5 0 011 0zm5 0a.5.5 0 11-1 0 .5.5 0 011 0z"
                                                        stroke="currentColor"></path></svg>
                                            </span>
                                        <input class="w-full border p-1 rounded text-xs" placeholder="Course Name"
                                               v-model="course[index].name">
                                    </div>
                                    <hr>
                                </template>

                                <template #item="{element, index}">
                                    <div class="border-1 border-b border-l border-r px-2 pb-2 bg-white flex  shadow">
                                        <p class="text-sm border-gray-100 cursor-move shadow p-2 bg-gray-50 rounded mt-2 w-full">
                                            {{ element.name }}</p>
                                    </div>
                                </template>


                            </draggable>
                        </div>
                    </template>
                </draggable>
            </div>

        </div>


    </div>
</template>


<script setup>
import draggable from "vuedraggable";
import {ref} from "vue";

const props = defineProps({
    categories: Object
})

const categoryData = (props.categories);

const handleChange = () => {
    console.log('changed');
}
const inputChanged = (value) => {
    console.log(value)
}
const getComponentData = () => {
    return {
        on: {
            change: handleChange,
            input: inputChanged
        },
        attrs: {
            wrap: true
        },
        props: {
            value: ['jkik']
        }
    };
}

const drag = ref(false)

// const categories = ref([
//     {name: "Eng", id: 1},
//     {name: "Bng", id: 2},
//     {name: "Math", id: 3},
//     {name: "Math 2", id: 4},
//     {name: "Math 3", id: 5},
//     {name: "Math 4", id: 6},
//     {name: "Math 5", id: 7},
//     {name: "Math 6", id: 8},
//     {name: "Math 7", id: 9},
// ])

let course = ref([])

const onChildDragStart = () => {
    console.log('Hello Start')
}
const onChildDragEnd = () => {
    console.log('Hello End')
}

const cloneDog = (data) => {
    console.log('sdsds')
    console.log(data)
    return data;
}

const start = (originalEvent) => {
    console.log(originalEvent)
}
const onCourseChange = (el) => {

    if (el.hasOwnProperty('added')) {
        const index = el['added'].newIndex
        let courseSubject = course.value[index].value ?? [];
        courseSubject.push({
            name: el['added'].element.name,
            id: el['added'].element.id
        })
        course.value[index] = {
            name: '',
            id: index,
            data: courseSubject
        }
    }
}

const log = (evt) => {
    window.console.log(evt);
}
</script>

<style scoped>
.ghost {
    background-color: red !important;
    height: fit-content !important;
}
</style>
