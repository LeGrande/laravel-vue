<template>
    <div>
        <CrudPageHeader title="Users" add-label="Add User" @add="modalRef.open()">
            <template #icon>
                <i class="bi bi-person-plus-fill me-1"></i>
            </template>
        </CrudPageHeader>

        <IndexTableState
            :error="error"
            :loading="loading"
            :is-empty="!users.length"
            loading-text="Loading users..."
            empty-text="No users found."
        >
            <div class="card shadow-sm">
                <IndexTable>
                    <template #headers>
                        <th scope="col">Last Name</th>
                        <th scope="col">First Name</th>
                        <th scope="col">Email</th>
                        <th scope="col" class="text-end">Actions</th>
                    </template>
                    <template #rows>
                        <IndexTableRow v-for="user in users" :key="user.id">
                            <IndexTableCell>{{ user.last_name }}</IndexTableCell>
                            <IndexTableCell>{{ user.first_name }}</IndexTableCell>
                            <IndexTableCell>{{ user.email }}</IndexTableCell>
                            <IndexTableCell class="text-end">
                                <AppButton
                                    variant="secondary"
                                    size="sm"
                                    outline
                                    class="me-1"
                                    @click="modalRef.open(user)"
                                    title="Edit user"
                                >
                                    <i class="bi bi-pencil-fill"></i>
                                </AppButton>
                                <AppButton
                                    variant="danger"
                                    size="sm"
                                    outline
                                    @click="handleDelete(user)"
                                    title="Delete user"
                                >
                                    <i class="bi bi-trash-fill"></i>
                                </AppButton>
                            </IndexTableCell>
                        </IndexTableRow>
                    </template>
                </IndexTable>
            </div>
        </IndexTableState>

        <UserFormModal
            ref="modalRef"
            :create-user="createUser"
            :update-user="updateUser"
        />

        <ConfirmModal
            ref="confirmRef"
            title="Delete User"
            confirm-button-text="Delete"
            confirm-variant="danger"
        />
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useUsers } from '../composables/useUsers.js';
import IndexTable      from '../components/table/IndexTable.vue';
import IndexTableRow   from '../components/table/IndexTableRow.vue';
import IndexTableCell  from '../components/table/IndexTableCell.vue';
import IndexTableState from '../components/table/IndexTableState.vue';
import UserFormModal   from '../components/users/UserFormModal.vue';
import ConfirmModal    from '../components/ConfirmModal.vue';
import AppButton       from '../components/AppButton.vue';
import CrudPageHeader  from '../components/CrudPageHeader.vue';

const { users, loading, error, fetchUsers, createUser, updateUser, deleteUser } = useUsers();

const modalRef   = ref(null);
const confirmRef = ref(null);

onMounted(fetchUsers);

async function handleDelete(user) {
    const confirmed = await confirmRef.value.open(
        `Are you sure you want to delete the ${user.first_name} ${user.last_name} user? This cannot be undone.`
    );
    if (!confirmed) return;

    try {
        await deleteUser(user.id);
    } catch (e) {
        error.value = e?.response?.data?.message ?? 'Failed to delete user.';
    }
}
</script>
