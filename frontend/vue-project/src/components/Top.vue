<script setup lang="ts">
const connect = async() => {
	const accouts = await window.ethereum.request({method: "eth_requestAccounts"})
	console.log(accouts[0]);
};
</script>

<template>
	<button @click="connect">Connect Metamask</button>
</template>
