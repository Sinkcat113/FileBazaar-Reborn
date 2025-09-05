<script>
    import {
        Button,
        Modal,
        ModalHeader,
        ModalBody,
        ModalFooter,
    } from "@sveltestrap/sveltestrap";

    export let success
</script>

<Modal isOpen={success}>
    <ModalHeader>
        <h4>Success</h4>
    </ModalHeader>
    <ModalBody>
        <p>Your files were transferred successfully!</p>
    </ModalBody>
    <ModalFooter>
        <Button
            color="light"
            on:click={() => {
                success = false;
            }}>Ok</Button
        >
    </ModalFooter>
</Modal>
