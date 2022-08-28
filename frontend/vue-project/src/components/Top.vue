<script setup lang="ts">
import { ethers } from 'ethers';
import gameitemsabi from "@/abi/gameitems.json";
import detectEthereumProvider from '@metamask/detect-provider';
import type { GameItems } from "@/abi/myERC1155.sol/index";
// const provider: any = await detectEthereumProvider();

const connect = async() => {
	const accouts = await window.ethereum.request({method: "eth_requestAccounts"})
	console.log(accouts[0]);
};
const getName = async() => {
	const provider = new ethers.providers.AlchemyProvider("goerli")
	console.log(provider)
	const gameItemsContract  = await new ethers.Contract("0xc06380D2867F3Ca42A0DA122643A41455dB2A9B6", gameitemsabi, provider) as GameItems;
	const GOLD = await gameItemsContract.GOLD()
	console.log(GOLD);
}
const getUri = async() => {
	const provider = new ethers.providers.AlchemyProvider("goerli")
	const gameItemsContract: GameItems = await new ethers.Contract("0xc06380D2867F3Ca42A0DA122643A41455dB2A9B6", gameitemsabi, provider)  as GameItems;
	const uri = await gameItemsContract.uri(2)
}
const getDeployerBalanceOf = async() => {
	const provider = new ethers.providers.AlchemyProvider("goerli")
	const gameItemsContract: GameItems = await new ethers.Contract("0xc06380D2867F3Ca42A0DA122643A41455dB2A9B6", gameitemsabi, provider)  as GameItems;
	const address = "0x32a9E70324862ef7BF8bA7610AF701822ddE5364"
	const balanceHex = await gameItemsContract.balanceOf(address,2)
	const balance = parseInt(String(balanceHex), 16)
	console.log(balance)
	// await gameItemsContract.
	// const uri = await gameItemsContract.uri(2)
}
</script>

<template>
	<button @click="connect">Connect Metamask</button>
	<button @click="getName">getName</button>
	<button @click="getUri">getUri</button>
	<button @click="getDeployerBalanceOf">getDeployerBalanceOf</button>
</template>
