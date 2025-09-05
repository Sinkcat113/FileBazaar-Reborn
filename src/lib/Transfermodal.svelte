<script>
    import {
        Modal,
        Progress,
        ModalHeader,
        Spinner,
    } from "@sveltestrap/sveltestrap";

    export let files
    export let open
    export let filesSent
</script>

<Modal isOpen={open}>
    <ModalHeader>
        <div style="display: flex; gap: 12px;">
            <Spinner color="primary"></Spinner>
            <h4>Sending files...</h4>
        </div>
    </ModalHeader>
    <Progress max={files.length} value={filesSent} />
</Modal>
