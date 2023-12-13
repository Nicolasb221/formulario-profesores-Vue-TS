<template>
    <section class="grid-container">
        <div class="grid-container__title">
            <h3>Añadir Profesor</h3>
        </div>
        <div class="grid-container__name"><input type="text" v-model="teacher.teacherName" placeholder="Nombre" id="Nombre" autocomplete="off"/></div>
        <div class="grid-container__surname"><input type="text" v-model="teacher.surname" placeholder="Apellidos" id="Apellidos" autocomplete="off"/></div>
        <div class="grid-container__id"><input type="number" v-model="teacher.id" placeholder="Identificacion" id="Identificacion" autocomplete="off"/></div>
        <div class="grid-container__subjects"><input type="text" v-model="subject" placeholder="Materias" id="Materias" autocomplete="off"/> <button id="btn-subject" @click="handleSubject()" >+</button></div>
        <div class="grid-container__list">
            <ul>
                <div class="grid-container__list-u" v-for="(elm, index) in teacher.subjects" :key="index">
                    <li >{{ elm }}</li>
                    <button class="grid-container__list-remove-sub" @click="handleRemoveSubject(index)">x</button>
                </div>
            </ul>
        </div>
        <div class="grid-container__check">
            <input id="btn-check" type="checkbox" v-model="teacher.doc"/> <span id="input-title">Documentación Entregada.</span>
            <button id="btn-checka" @click="handleAddTeacher()">Agregar</button>
        </div>
        <div class="grid-container__list-teacher"><h3>Listado de profesores</h3>
            <div class="grid-container__2">
                <div class="grid-container__2-container" v-for="(elm, index) in teachers" :key="elm.id">
                    <div id="remove-teacher"><button id="btn-remove-teacher" @click="handleRemoveTeacher(index)">x</button></div>
                    <div class="grid-container__2-items"><b>Nombre: </b>{{ elm.teacherName }}</div> 
                    <div class="grid-container__2-items"><b>Apellidos: </b>{{ elm.surname }}</div>
                    <div class="grid-container__2-items"><b>ID: </b>{{ elm.id }}</div>
                    <ul id="lista-materias">
                        <li v-for="(item, index) in elm.subjects" :key="index"><b>Materia {{index + 1}}: </b> {{ item }}</li>
                    </ul>
                    <div class="grid-container__2-items" v-if="elm.doc"><b>Documentación: </b> Entregada</div>
                    <div class="grid-container__2-items" v-else><b>Documentación: </b> No entregada</div>
                </div>

            </div>
        </div>

    </section>

</template>

<script lang="ts" setup>
    import {Ref, ref} from 'vue';
    interface ITeacher{
        teacherName: string,
        surname: string,
        id: string | number,
        subjects: Array<string>,
        doc: boolean
    }
    let teacher:Ref<ITeacher> = ref({
        teacherName: '',
        surname: '',
        id:'',
        subjects:[],
        doc:false
    })

    let teachers:Ref<Array<ITeacher>> = ref([]);
    let subject:Ref<string> = ref('')

    const handleSubject = () =>{
        if(subject.value!==''){
            teacher.value.subjects.push(subject.value);
            subject.value='';
        }else alert("Ingrese una materia")
    }
    const handleRemoveSubject = (i:number) =>{
        teacher.value.subjects.splice(i,1)
    }
    const handleAddTeacher = () => {
        if(teacher.value.teacherName!=="" && teacher.value.surname!=="" && teacher.value.id!=="" && teacher.value.subjects.length >= 1){
            teachers.value.push({
                teacherName:teacher.value.teacherName,
                surname:teacher.value.surname,
                id:teacher.value.id,
                subjects:teacher.value.subjects,
                doc:teacher.value.doc
            });
            teacher.value.teacherName=""
            teacher.value.surname=""
            teacher.value.id=""
            teacher.value.subjects=[]
            teacher.value.doc= false;
        }else alert("ingresa todos los datos")
    }
    const handleRemoveTeacher = (i:number) =>{
        teachers.value.splice(i,1);
    }
</script>

<style scoped>
.grid-container{
    display: grid;
    grid-template-areas:
    "title title title" 
    "name surname id"
    "materias materias materias"
    "list list list"
    "check check check"
    "c c c";
    grid-template-rows: 80px 10vh 10vh 20vh 10vh 1fr;
    grid-template-columns: 1fr 1fr 1fr;
    max-width: 50vw;
    margin: auto;
    background-color: #e1e3ac90;
    border-radius: 15px;
    padding: 10px;
    margin-top: 10vh;
}
.grid-container__title{
    grid-area: title;
    display: flex;
    align-items: center;
    place-content: center;
}
.grid-container__title > h3{
    color: #46685b;
    font-size: 40px;
    
}
.grid-container__name{
    grid-area: name;
    display: flex;
    align-items: center;
    place-content: center;
}
.grid-container__surname{
    grid-area: surname;
    display: flex;
    align-items: center;
    place-content: center;
}
.grid-container__id{
    grid-area: id;
    display: flex;
    align-items: center;
    place-content: center;
}
input[type=number]::-webkit-inner-spin-button, input[type=number]::-webkit-outer-spin-button { 
  -webkit-appearance: none; 
  margin: 0; 
}
.grid-container__subjects{
    grid-area: materias;
    display: flex;
    align-items: center;
    place-content: center;
    gap: 20px;
}
.grid-container__list{
    grid-area: list;
    background: linear-gradient(to top, #a6b985, transparent);
    border-radius: 15px;
    margin:0 20px;
    overflow-y: auto;
}
.grid-container__list > ul{
    display: flex;
    gap: 5px;
    flex-wrap: wrap;
}
.grid-container__check{
    grid-area: check;
    display: flex;
    align-items: center;
    justify-content: space-evenly;
}
.grid-container__list-u > li{
    display: inline-block;
    margin:5px 10px;
    font-size: 12px;
    color: #46685b;
    font-weight: 700;
}
.grid-container__list::-webkit-scrollbar {
  -webkit-appearance: none;
}
.grid-container__list-teacher{
    grid-area: c;
    text-align: center;
    color: #46685b;
}

.grid-container__list-remove-sub{
    margin: 5px;
    border-style: none;
    color: rgb(220, 20, 60);
    font-weight: 900;
    background-color: transparent;
    cursor: pointer;
    font-size: 15px;
}
.grid-container__list-u{
    background: linear-gradient(to top, #46685b99, transparent);
    border-radius: 10px;
}
#Nombre, #Apellidos, #Identificacion, #Materias{
    color: #46685b;
    font-size: 12px;
    border-style: none;
    background-color: transparent;
    border-bottom: 2px solid #e1e3ac;
    padding: 5px;
    max-width: 15vw;
}
#Nombre::placeholder, #Apellidos::placeholder, #Identificacion::placeholder, #Materias::placeholder{
    color: #46685b;
}
#Nombre:focus, #Apellidos:focus, #Identificacion:focus, #Materias:focus{
    outline: none;
    background: linear-gradient(to top, #a6b9857b, transparent);
}
#btn-subject{
    background-color: transparent;
    border-style: none;
    color:#46685b;
    font-size: 25px;
    font-weight: 900;
    cursor: pointer;
}
#btn-check{
    cursor: pointer;
}
#btn-checka{
    background: linear-gradient(to top, #a6b985, transparent);
    color: #46685b;
    border-style: none;
    border-radius: 10px;
    font-size: 14px;
    font-weight: 500;
    padding: 8px;
    cursor: pointer;
}
#input-title{
    color: #46685b;
    font-size: 14px;
}
.grid-container__2{
    display: grid;
    grid-auto-rows:1fr;
    grid-template-columns: 1fr 1fr;
    margin: 20px;
    padding: 10px;
    gap: 20px;
}
.grid-container__2-items{
    display: flex;
    justify-content: center;
}
#lista-materias{
    list-style-type: none;
    padding: 0;
}
.grid-container__2-container{
    background-color: #a6b985ad;
    border-radius: 15px;
    padding: 8px;
}
#remove-teacher{
    display:flex;
    justify-content: right;

}
#btn-remove-teacher{
    border-style: none;
    color: rgb(220, 20, 60);
    font-weight: 900;
    background-color: transparent;
    cursor: pointer;
    font-size: 15px;
}
@media screen and (max-width: 682px) {
    .grid-container{
        max-width: 80vw;
    }
    .grid-container__2{
        grid-template-columns: 1fr;
    }
}
@media screen and (max-width: 404px) {
    .grid-container__title > h3{
        font-size: 30px;
    }
    #input-title{
        font-size: 12px;
    }
}
</style>    