<template>
    <div>
        <h2>학과 상세 조회</h2>
        <!--  
        <p>{{ currentRoute.fullPath }}</p>
        <p>{{ currentRoute.path }}</p>
        <p>{{ currentRoute.params }}</p>
        <p>{{ currentRoute.params.no }}</p>
        <p>{{ currentRoute.query }}</p>
        <p>{{ currentRoute.query.name }}</p>
        <p>{{ parseInt(currentRoute.query.age)}}</p>
        -->
        <DepartmentForm submitButtonText="학과 수정" :initFormData="initFormData"
            @form-submit="formSubmit"/>
    </div>
</template>

<script setup>
    import apiClient from '@/api';
    import DepartmentForm from '@/components/DepartmentForm.vue';
    import { onMounted, ref } from 'vue';
    import { useRoute } from 'vue-router';

    const currentRoute = useRoute();
    const initFormData = ref({
        name: '',
        category: '',
        openYn: '',
        capacity: 0
    });

    const fetchDepartment = async (departmentNo) => {
        try {
            const response = await apiClient.get(`/departments/${departmentNo}`);

            console.log(response);

            initFormData.value = response.data.result;
        } catch (error) {
            console.log(error);
        }
    }

    const formSubmit = async (formData) => {
        try {
            const response = await apiClient.put(`/departments/${formData.no}`, formData);

            if (response.data.code === 200) {
                alert('정상적으로 수정되었습니다.')
            }
        } catch (error) {
            console.log(error);
        }
    };

    onMounted(() => {
        fetchDepartment(currentRoute.params.no);
    });
</script>

<style scoped>

</style>