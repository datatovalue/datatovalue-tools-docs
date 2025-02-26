<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
</script>

<EvidenceDefaultLayout {data} hideTOC= true title="Data to Value Tools">
	<slot slot="content" />
</EvidenceDefaultLayout>


