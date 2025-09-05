<script>
    import {
        Button,
        Modal,
        ModalHeader,
        ModalBody,
        ModalFooter,
    } from "@sveltestrap/sveltestrap";

    export let failed
</script>

<Modal isOpen={failed}>
    <ModalHeader>
        <h4>Uh oh :/</h4>
    </ModalHeader>
    <ModalBody>
        <p>Something went wrong!</p>
    </ModalBody>
    <ModalFooter>
        <Button
            color="dark"
            on:click={() => {
                failed = false;
            }}>Ok</Button
        >
    </ModalFooter>
</Modal>

<style>
    h4 {
        margin: 5px;
    }

    p {
        margin: 5px;
    }
</style>