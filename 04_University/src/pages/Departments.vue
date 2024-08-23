<template>
    <div>
        <h2>학과 목록 조회</h2>

        <DepartmentList :departments="departments"
            @delete-department="deleteDepartment"
            @item-click="itemClick"/>
        <PageNation :currentPage="currentPage" :startPage="startPage" :endPage="endPage"
            @change-page="changePage"/>
    </div>
</template>

<script setup>
    import DepartmentList from '@/components/DepartmentList.vue';
    import PageNation from '@/components/PageNation.vue';
    import apiClient from '@/api';
    import { onMounted, ref, watch } from 'vue';
    import { useRoute, useRouter } from 'vue-router';

    const departments = ref([]);
    const currentRoute = useRoute();
    const router = useRouter();

    // 페이지네이션을 위한 데이터
    const currentPage = ref(parseInt(currentRoute.query.page) || 1); // 현재 페이지
    const startPage = ref(0); // 페이징 된 페이지 중 시작 페이지 
    const endPage = ref(0); // 페이징 된 페이지 중 마지막 페이지
    const maxPage = ref(0); // 전체 페이지 중 마지막 페이지
    const pageLimit = 5; // 페이지네이션에 보이는 페이지 수
    const listLimit = 10; // 한 페이지에 표시될 리스트의 수


    // axios 사용 방법
    // const fetchDepartments = () => {
    //     apiClient.get('/departments?page=1&numOfRows=10')
    //             .then((response) => {
    //                 console.log(response);
    //             })
    //             .catch((error) => {
    //                 console.log(error);
    //             });
    // };

    // async / await
    const fetchDepartments = async (page) => {
        try {
            const response = await apiClient.get(`/departments?page=${page}&numOfRows=10`);

            departments.value = response.data.result;

            /*
            totalCount = 100, listLimit = 10

            100 / 10 = 10   => 10 페이지
            101 / 10 = 10.1 => 11 페이지
            103 / 10 = 10.3 => 11 페이지
            112 / 10 = 11.2 => 12 페이지
            */
            maxPage.value = parseInt(Math.ceil(response.data.totalCount / listLimit));

            /*
                < 1 2 3 4 5 6 7 8 9 10>
                < 11 12 13 14 15 16 17 18 19 20>
                < 21 22 23 24 25 26 27 28 29 30>

                1, 11, 21, 31, ... => (10 * n) + 1 (n >= 0)

                1 ~ 10  => n = 0
                11 ~ 20 => n = 1
                21 ~ 30 => n = 2
                ...

                n = (currentPage - 1) / pageLimit

                (10 * ((currentPage - 1) / pageLimit)) + 1 (n >= 0)
            */
            startPage.value = (pageLimit * parseInt((page - 1) / pageLimit)) + 1;

            // 10, 20, 30, 40, ...
            endPage.value = startPage.value + pageLimit - 1;
            endPage.value = endPage.value > maxPage.value ? maxPage.value : endPage.value;
        } catch (error) {
            console.log(error);
        }
    };

    const changePage = (page) => {
        if (page >= 1 && page <= maxPage.value) {
            router.push({name: 'departments', query: {page}});
        }
    };

    const deleteDepartment = async (departmentNo) => {
        try {
            const response = await apiClient.delete(`/departments/${departmentNo}`);

            if (response.data.code === 200) {
                alert('정상적으로 삭제되었습니다.');

                fetchDepartments(currentPage.value);
            } else if(response.data.code === 404) {
                alert('존재하지 않는 학과입니다.');
            } else {
                alert('학과 삭제를 실패했습니다.');
            }
        } catch (error) {
            console.log(error);
        }
    };

    const itemClick = (departmentNo) => {
        router.push({name: 'departments/no', params: {no: departmentNo}});
    };

    onMounted(() => {
        fetchDepartments(currentPage.value);
    });

    watch(currentRoute, () => {
        currentPage.value = parseInt(currentRoute.query.page) || 1;

        fetchDepartments(currentPage.value);
    });
</script>``

<style scoped>

</style>