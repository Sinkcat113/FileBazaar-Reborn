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
    <ListGroup>
        {#each recievedFiles as rf}
            <ListGroupItem color="success" tag="button">
                <Row>
                    <Col xs="auto">
                        <Icon name="file-earmark-fill" />
                    </Col>
                    <Col>
                        {rf.file.name}
                    </Col>
                    <Col xs="auto">
                        <a download={rf.file.name} href={rf.url}>
                            <Icon name="download" />
                        </a>
                    </Col>
                </Row>
            </ListGroupItem>
        {/each}
    </ListGroup>
</div>

<style>
    .file-list {
        margin-top: 60px;
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
</style>
