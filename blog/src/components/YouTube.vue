<template>
    <div>{{ UpperCase(info.firstName) }}
        <h1>Rs {{ CurrencyConver(1131) }}</h1>
    </div>
</template>
<script>
export default{
    data(){
        return{
            info:{
                firstName: 'Ali',
                lastName: 'Ahmad',
            }
        }
    },
    methods:{
        UpperCase(value){
            return value.toUpperCase();
        },
        CurrencyConver(value){
            return (73.96*value).toFixed(2);
        }
    }
}
</script>