<template>
    <div>
        <div class="pl-2 pr-2 d-md-none d-block">
            <table class="table" style="background-color: white">
                <thead>
                    <tr>
                        <th scope="col">number</th>
                        <th scope="col">detail</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(participant,index) in filteredParticipant" :key="`participant-${index}`">
                        <th>
                            {{participant.id}}
                        </th>
                        <td>
                            <h5>
                                name: {{participant.data.name}}
                            </h5>
                            <h5>
                                surname: {{participant.data.surname}}
                            </h5>
                            <h5>
                                studentCode: {{participant.data.studentCode}}
                            </h5>
                            <a :href="`tel:${participant.data.phoneNumber}`">
                                tel: {{participant.data.phoneNumber}}
                            </a>
                        </td>
                        
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="pl-2 pr-2 d-md-block d-none">
            <table class="table" style="background-color: white">
                <thead>
                    <tr>
                        <th scope="col">Number</th>
                        <th scope="col">Name</th>
                        <th scope="col">Surname</th>
                        <th scope="col">Student Code</th>
                        <th scope="col">Contact Number</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(participant,index) in filteredParticipant" :key="`participant-${index}`">
                        <th>
                            {{participant.id}}
                        </th>
                        <td>
                                {{participant.data.name}}
                        </td>
                        <td>
                                {{participant.data.surname}}
                        </td>
                        <td>
                                {{participant.data.studentCode}}
                        </td>
                        <td>
                            <a :href="`tel:${participant.data.phoneNumber}`">
                                {{participant.data.phoneNumber}}
                            </a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
            
    </div>
</template>
<script>
export default {
    props:{
        filteredParticipant:{}
    }
}
</script>