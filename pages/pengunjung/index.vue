<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-12">
                <h2 class="tex-center my-4">RIWAYAT KUNJUNGAN</h2>
                <div class="my-3">
                    <form @submit.prevent="getpengunjung"> 
                    <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5" placeholder="Filter..."/>
                    </form>
                </div>
                <div class="my-3 tex-muted">menampilkan {{visitors.length }} dari {{jumlah}}</div>
                <table class="table table-bordered">
                    <thead>
                        <tr text-center>
                            <td>NO</td>
                            <td>NAMA</td>
                            <td>KEANGGOTAAN</td>
                            <td>WAKTU</td>
                            <td>KEPERLUAN</td>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(visitor,i) in visitors" :key="i">
                            <td>{{ i+1}} .</td>
                            <td>{{ visitor.nama }}</td>
                            <td>{{ visitor.keanggotaan.nama }}</td>
                            <td>{{ visitor.tanggal }}, {{ visitor.waktu}}</td>
                            <td>{{ visitor.keperluan.nama }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <nuxt-link to="../"><button type="submit" class="btn btn-lg rounded-4 px-5 bg-danger text-black" style="float: right;">kembali</button></nuxt-link>
        </div>
    </div>
</template>

<script setup>
useHead({
    title:'aplikasi perpus digital',
    meta:[{name:'description', content:'selamat mengujungi'}]
})
const supabase = useSupabaseClient();
const keyword = ref('')
const visitors = ref ([])
const jumlah = ref ([])

const getpengunjung = async () => {
    const { data, error } = await supabase.from("pengunjung").select(`*, keanggotaan(*), keperluan(*)`)
        .ilike('nama', `%${keyword.value}%`)
        .order(`id`, {ascending: false});
    if(data) visitors.value = data
};

const totalpengunjung = async () => {
    const {data, count} = await supabase.from ('pengunjung').select("*", {count: 'exact'});
    if (data) jumlah.value = count;
};
onMounted(() => {
    getpengunjung()
    totalpengunjung()
})
</script>

<style scoped>
.col-lg-12 {
    text-align: center;
}
</style>