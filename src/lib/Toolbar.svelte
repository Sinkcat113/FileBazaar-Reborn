<script>
    import {
        Button,
        ButtonGroup,
        Icon,
        Input,
        InputGroup,
        Popover,
    } from "@sveltestrap/sveltestrap";

    export let id;
    export let address;

    export let handleCopy;
    export let handleInput;
    export let handleRemove;
    export let handleTransfer;
</script>

<div class="toolbar">
    <ButtonGroup>
        <Button color="light" on:click={handleInput}>
            <Icon name="folder-plus" />
        </Button>
        <Button color="light" on:click={handleRemove}>
            <Icon name="folder-x" />
        </Button>
    </ButtonGroup>
    <InputGroup>
        <Input type="text" disabled value={id} />
        <Popover target="btn-copy" hideOnOutsideClick>Copied!</Popover>
        <Button color="light" id="btn-copy" on:click={handleCopy}>
            <Icon name="copy" />
        </Button>
    </InputGroup>
    <InputGroup>
        <Input type="text" placeholder="Address" bind:value={address} />
        <Button color="primary" on:click={handleTransfer}>
            <Icon name="folder-symlink-fill" />
        </Button>
    </InputGroup>
</div>

<style>
    .toolbar {
        display: flex;
        gap: 12px;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        padding: 12px;
        background-color: white;
        border-bottom-style: solid;
        border-width: 1px;
        border-color: rgba(22, 22, 28, 0.113);
        z-index: 5;
    }
</style>
