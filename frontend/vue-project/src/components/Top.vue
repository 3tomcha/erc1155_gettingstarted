<script setup lang="ts">
import { BigNumber, ethers } from 'ethers';
import gameitemsabi from "@/abi/gameitems.json";
import detectEthereumProvider from '@metamask/detect-provider';
import type { GameItems } from "@/abi/myERC1155.sol/index";
// const provider: any = await detectEthereumProvider();

const contractAddress = "0xc06380D2867F3Ca42A0DA122643A41455dB2A9B6";
const deployerAddress = "0x32a9E70324862ef7BF8bA7610AF701822ddE5364";
const playerAddress = "0x3D896D141dC4eEe51E829CcA7003939be20c280A";


const connect = async() => {
	const accouts = await window.ethereum.request({method: "eth_requestAccounts"})
	console.log(accouts[0]);
};
const getName = async() => {
	const provider = new ethers.providers.AlchemyProvider("goerli")
	console.log(provider)
	const gameItemsContract  = await new ethers.Contract(contractAddress, gameitemsabi, provider) as GameItems;
	const GOLD = await (await gameItemsContract.GOLD()).toString()
	console.log(GOLD);
}
const getUri = async() => {
	const provider = new ethers.providers.Web3Provider(window.ethereum)
	const signer = await provider.getSigner();
	const gameItemsContract: GameItems = await new ethers.Contract(contractAddress, gameitemsabi, signer)  as GameItems;
	const uri = await gameItemsContract.uri(2)
	console.log(uri);
}
const getDeployerBalanceOf = async() => {
	const provider = new ethers.providers.AlchemyProvider("goerli")
	const gameItemsContract: GameItems = await new ethers.Contract(contractAddress, gameitemsabi, provider)  as GameItems;
	const address = deployerAddress
	const balanceHex = await gameItemsContract.balanceOf(address,0)
	const balance = balanceHex.toString()
	console.log(balance)
	// const balance = parseInt(String(balanceHex), 16)
	// console.log(financial(balance))
	// await gameItemsContract.
	// const uri = await gameItemsContract.uri(2)
}
const getBalanceOfBatch = async() => {
	const provider = new ethers.providers.AlchemyProvider("goerli")
	const gameItemsContract: GameItems = await new ethers.Contract(contractAddress, gameitemsabi, provider)  as GameItems;
	const address = deployerAddress
	const balanceHex = await gameItemsContract.balanceOfBatch([address, address, address, address, address], [1, 2, 3, 4, 5])
	// const balance = balanceHex.map(hex => parseInt(String(hex), 16));
	// console.log(balanceHex)
	// await gameItemsContract.
	// const uri = await gameItemsContract.uri(2)
}
const financial = (x:number) => {
  return Number.parseFloat(String(x)).toFixed(2);
}
const safeBatchTransferFrom = async () => {
	// const provider = new ethers.providers.AlchemyProvider("goerli")
	const provider = new ethers.providers.Web3Provider(window.ethereum)
	const signer = await provider.getSigner();
	const gameItemsContract: GameItems = await new ethers.Contract(contractAddress, gameitemsabi, signer)  as GameItems;
	const res = await gameItemsContract.safeBatchTransferFrom(deployerAddress, playerAddress, [0, 1, 3, 4], [50, 100, 1, 1], "0x00").catch(err => {
		console.error(err)
	})
}
const balanceOfBatch = async () => {
	const provider = new ethers.providers.Web3Provider(window.ethereum)
	const signer = await provider.getSigner();
	const gameItemsContract: GameItems = await new ethers.Contract(contractAddress, gameitemsabi, signer)  as GameItems;
	const res = await gameItemsContract.balanceOfBatch([playerAddress, playerAddress, playerAddress, playerAddress, playerAddress], [0, 1, 2, 3, 4]).catch(err => {
		console.error(err)
	})
	if (res) {
		const result = res.map(bignumber => bignumber.toString())
		console.log(result);
	}
}
</script>

<template>
	<button @click="connect">Connect Metamask</button>
	<button @click="getName">getName</button>
	<button @click="getUri">getUri</button>
	<button @click="getDeployerBalanceOf">getDeployerBalanceOf</button>
	<button @click="getBalanceOfBatch">getBalanceOfBatch</button>
	<button @click="safeBatchTransferFrom">safeBatchTransferFrom</button>
	<button @click="balanceOfBatch">balanceOfBatch</button>
</template>
