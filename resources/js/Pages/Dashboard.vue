<script setup>
import GameCard from '@/Shared/GameCard';
import DashboardBalanceCard from '@/Shared/DashboardBalanceCard/DashboardBalanceCard';
import DashboardBalanceCardCreateAccount from '@/Shared/DashboardBalanceCard/DashboardBalanceCardCreateAccount';
import ButtonShape from '@/Shared/ButtonShape';
import TentModal from '@/Shared/Modals/TentModal';

import BorderedContainer from '@/Shared/BorderedContainer';
import { ref, reactive } from 'vue';
import { isEmpty } from 'lodash';
import { Inertia } from '@inertiajs/inertia';

let props = defineProps({
    dashboard_art: String,
    games: Object,
    balance: Array,
    user: Object,
});

let playGameModalIsOpen = ref(false);
let playGameModalSelectedGame = reactive({});

function playGameModalCancelButtonClicked() {
    playGameModalIsOpen.value = false;
    playGameModalSelectedGame = {};
}

function playGameModalStartButtonClicked() {
    if (isEmpty(playGameModalSelectedGame)) {
        return;
    }
    console.log('starting..', playGameModalSelectedGame.name);
    playGameModalIsOpen.value = false;
    Inertia.visit('/games/1');
}

function gameActionButtonClicked(game) {
    playGameModalSelectedGame = game;
    playGameModalIsOpen.value = true;
}
</script>

<template>
    <div class="flex h-full flex-col lg:flex-row lg:space-x-6">
        <div class="w-full lg:w-3/4">
            <BorderedContainer
                class="mb-8 flex flex-col space-y-6 border-wgh-red-3 bg-wgh-red-2 p-6 md:flex-row md:space-x-6 md:space-y-0"
            >
                <div class="w-full md:w-1/2">
                    <img :src="props.dashboard_art" alt="Dashboard Art" />
                </div>
                <div class="flex w-full flex-col justify-center md:w-1/2">
                    <h2 class="font-grota text-2xl font-extrabold text-white">
                        The First Retro Gaming Playground Competitions!
                    </h2>
                    <p class="mt-4 font-inter text-base font-normal text-white">
                        A small description about the first section explaining
                        about the platform.
                    </p>
                </div>
            </BorderedContainer>

            <h1 class="mb-6 font-grota text-2xl font-extrabold text-wgh-gray-6">
                Games
            </h1>
            <TentModal :open="playGameModalIsOpen">
                <template v-slot:header><p>Hello!</p></template>
                <template v-slot:title>
                    <p>A small description about the first section.</p>
                </template>
                <template v-slot:subtitle>
                    <p class="wgh-gray-6 font-inter text-base font-normal">
                        A small description about the first section explaining
                        about the platform.
                    </p>
                </template>
                <template v-slot:actions>
                    <button @click.prevent="playGameModalCancelButtonClicked">
                        <ButtonShape type="gray">
                            <span>Cancel</span>
                        </ButtonShape>
                    </button>
                    <button @click.prevent="playGameModalStartButtonClicked">
                        <ButtonShape type="red">
                            <span>Start</span>
                        </ButtonShape>
                    </button>
                </template>
            </TentModal>
            <div>
                <GameCard
                    @actionButtonClicked="gameActionButtonClicked(game)"
                    v-for="game in props.games.data"
                    :key="game.id"
                    :game="game"
                />
            </div>
        </div>
        <div class="h-full w-full space-y-6 lg:w-1/4">
            <DashboardBalanceCard v-if="user" :balance="balance" />
            <DashboardBalanceCardCreateAccount v-if="!user" />
        </div>
    </div>
</template>
