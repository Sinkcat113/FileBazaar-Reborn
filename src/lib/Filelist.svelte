<script>
    import {
        Icon,
        ListGroup,
        ListGroupItem,
        Row,
        Col,
        Container,
    } from "@sveltestrap/sveltestrap";

    export let files;
    export let recievedFiles;
</script>

<div class="file-list">
    {#if files.length > 0}
        <h4>Outgoing Files <Icon name="cloud-arrow-up" /></h4>
    {/if}
    <ListGroup>
        {#if files.length <= 0 && recievedFiles.length <= 0}
            <div class="drag-n-drop-msg">
                <Container>
                    <Row>
                        <Col sm="12" md={{ size: 6, offset: 3 }}>
                            <span
                                >Click the <Icon name="folder-plus" /> icon to add
                                files.</span
                            >
                        </Col>
                    </Row>
                </Container>
            </div>
        {/if}
        {#each files as f}
            <ListGroupItem color="link" tag="button">
                <Row>
                    <Col xs="auto">
                        <Icon name="file-earmark-fill" />
                    </Col>
                    <Col>
                        {f.name}
                    </Col>
                </Row>
            </ListGroupItem>
        {/each}
    </ListGroup>
    {#if recievedFiles.length > 0}
        <h4>Recieved Files <Icon name="cloud-arrow-down" /></h4>
    {/if}
    <ListGroup>
        {#each recievedFiles as rf}
            <a download={rf.file.name} href={rf.url}>
                <ListGroupItem color="link" tag="button">
                    <Row>
                        <Col xs="auto">
                            <Icon name="file-earmark-fill" />
                        </Col>
                        <Col>
                            {rf.file.name}
                        </Col>
                        <Col xs="auto">
                            <Icon name="download" />
                        </Col>
                    </Row>
                </ListGroupItem>
            </a>
        {/each}
    </ListGroup>
</div>

<style>
    .file-list {
        margin-top: 43px;
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .drag-n-drop-msg {
        text-align: center;
        margin-top: 25dvh;
        padding: 12px;
        margin-left: auto;
        margin-right: auto;
        transition: 0.12s;
    }

    h4 {
        margin: 0;
        margin-top: 23px;
    }

    a {
        color: black;
        text-decoration: inherit;
    }

    a:hover {
        text-decoration: underline;
    }
</style>
