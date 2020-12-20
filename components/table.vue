<template>
    <div class="pl-2 pr-2">
        <table class="table" style="background-color: white">
            <thead>
                <tr>
                    <th scope="col">id</th>
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
</template>
<script>
export default {
    props:{
        filteredParticipant:{}
    }
}
</script>