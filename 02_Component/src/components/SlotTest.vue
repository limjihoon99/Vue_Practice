<template>
    <div>
        <!-- 슬롯 테스트 -->
        <!-- 
        <SlotItem v-for="item in items" :key="item.id"
            :id="item.id" :checked="item.checked">
            자식 컴포넌트의 <slot> 영역에 출력된다.
            <span v-if="item.checked" v-text="item.label"></span>
            <span v-else v-text="item.label" style="color: gray;"></span>
        </SlotItem> 
        -->

        <!-- 명명된 슬롯 테스트 -->
        <!-- 
        <SlotItem v-for="item in items" :key="item.id"
            :id="item.id" :checked="item.checked" @checkChange="checkChange">
            <template v-slot:icon>
                <span v-if="item.checked" class="material-symbols-outlined">check</span>
                <span v-else class="material-symbols-outlined">close</span>
            </template>

            <template v-slot:label> 
                <template v-slot:default>
                    <span v-if="item.checked" v-text="item.label"></span>
                    <span v-else v-text="item.label" style="color: gray;"></span>
                </template>
            </SlotItem>
            -->

        <SlotItem v-for="item in items" :key="item.id" :item="item" @checkChange="checkChange">
            <template v-slot:icon>
                <span v-if="item.checked" class="material-symbols-outlined">check</span>
                <span v-else class="material-symbols-outlined">close</span>
            </template>
                
            <template v-slot:default>
                <span v-if="item.checked" v-text="item.label"></span>
                <span v-else v-text="item.label" style="color: gray;"></span>
            </template>
        </SlotItem>
    </div>
</template>

<script>
    import SlotItem from './SlotItem.vue';

    export default {
        name: 'SlotTest',
        components: {
            SlotItem,
        },
        data() {
            return {
                items: [
                    {id: 'V', checked: true, label: 'Vue'},
                    {id: 'A', checked: false, label: 'Angular'},
                    {id: 'R', checked: false, label: 'React'}
                ]
            }
        },
        methods: {
            checkChange(e) {
                console.log(e);

                const item = this.items.find((item) => item.id === e.id);

                item.checked = e.checked;
            }
        },
    }
</script>