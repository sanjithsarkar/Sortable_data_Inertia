<template>
    <div class="grid grid-cols-6 gap-4 p-4">
        <div class="col-span-3 md:col-span-1 flex flex-col gap-2 shadow rounded p-4">
            <h3 class="text-[16px]">Subjects</h3>
            <hr class="m-0">
            <div>
                <draggable
                    v-model="categoryData"
                    group="people"
                    @start="drag=true"
                    @end="drag=false"
                    item-key="id">
                    <template #item="{element}">
                        <p class="text-sm border-1 border-b mt-2 border-gray-200 cursor-move">{{element.name}}</p>
                    </template>
                </draggable>
            </div>
        </div>

        <div class="col-span-3 md:col-span-5 flex flex-col gap-2">
            <h3>Courses</h3>
                <div class="border rounded p-4">
                    <draggable
                        class="grid grid-cols-1 lg:grid-cols-4 w-full bg-gray-100 gap-4 pb-4 lg:pb-0"
                        handle=".handle"
                        v-model="course"
                        @change="onCourseChange"
                        group="people"
                        item-key="id">
                        <template #item="{element, index}">
                            <div class="flex flex-col">
                                <div class="h-5 w-full cursor-move">
                                    <!-- <span class="handle">Move </span> -->
                                </div>
                                <input class="border p-1 rounded text-xs" v-model="course[index].name">
                                <draggable
                                    class="w-full bg-gray-300 pt-4 mr-4 h-full"
                                    v-model="course[index].data"
                                    group="people"
                                    @start="onChildDragStart"
                                    @end="onChildDragEnd"
                                    item-key="id">
                                    <template #item="{element, index}">
                                        <div class="m-2 rounded border-1 border-b bg-white">
                                            <span class="p-3">{{element.name}}</span>
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
import {ref, defineProps} from "vue";
const drag = ref(false)

// const categories = ref([
//     {name: "Eng", id: 1},
//     {name: "Bng", id: 2},
//     {name: "Math", id: 3},
// ])

const props = defineProps({
    categories: Object
})

const categoryData = (props.categories);

let course = ref([

])

const onChildDragStart = ()=>{
    console.log('Hello Start')
}
const onChildDragEnd = ()=>{
    console.log('Hello End')
}

const cloneDog = (data) => {
    console.log('sdsds')
    console.log(data)
    return data;
}

const start = ( originalEvent ) => {
    console.log(originalEvent)
}
const onCourseChange = (el)=>{

    if(el.hasOwnProperty('added')){
        const index = el['added'].newIndex
        let courseSubject = course.value[index].value??[];
        courseSubject.push({
            name: el['added'].element.name,
            id: el['added'].element.id
        })
        course.value[index] = {
            name:'New Course',
            id: index,
            data: courseSubject
        }
    }
}

const log =  (evt) => {
    window.console.log(evt);
}
</script>


