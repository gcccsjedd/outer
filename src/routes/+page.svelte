<script lang="ts">
	$effect(() => {
		if (typeof window !== 'undefined' && 'BroadcastChannel' in window) {
			const receivingChannel = new BroadcastChannel('testingChannel');
			const sendingChannel = new BroadcastChannel('testingChannel');

			console.log('from Child');
			const timer = setInterval(() => {
				sendingChannel.postMessage('This is a test message. From OUTER');
			}, 5000);

			receivingChannel.onmessage = (event) => {
				const message = event.data;
				console.log("OUTER RECEIVING",message);
			};

			return () => {
				receivingChannel.close();
				sendingChannel.close();
				clearInterval(timer);
			};
		}
	});
</script>

<div class=" h-screen w-screen bg-black"></div>
