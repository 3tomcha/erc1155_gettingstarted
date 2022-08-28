<script setup lang="ts">
import { ethers } from 'ethers';
import gameitemsabi from "@/abi/gameitems.json";
import detectEthereumProvider from '@metamask/detect-provider';
// const provider: any = await detectEthereumProvider();

const connect = async() => {
	const accouts = await window.ethereum.request({method: "eth_requestAccounts"})
	console.log(accouts[0]);
};
const getName = async() => {
	const provider = new ethers.providers.AlchemyProvider()
	console.log(provider)
	const gameItemsContract = await new ethers.Contract("0x8a5EdfF6358E2F8F9a635a70ffa0746f3C60d785", gameitemsabi, provider);
	const address = await gameItemsContract.address
	console.log(address);
}
</script>

<template>
	<button @click="connect">Connect Metamask</button>
	<button @click="getName">getName</button>
</template>
