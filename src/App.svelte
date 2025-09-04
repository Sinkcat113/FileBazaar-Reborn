<script>
  // @ts-nocheck

  import {
    Button,
    ButtonGroup,
    Icon,
    Input,
    InputGroup,
    ListGroup,
    Styles,
    ListGroupItem,
    Row,
    Col,
    Container,
    Popover,
  } from "@sveltestrap/sveltestrap";

  import { Peer } from "peerjs";
  import Toolbar from "./lib/Toolbar.svelte";
  import Filelist from "./lib/Filelist.svelte";

  let peer = new Peer();
  let id;
  let address;

  peer.on("open", (iD) => {
    id = iD;
  });

  $: files = [];
  $: recievedFiles = [];
  let fileInput;

  peer.on("connection", (conn) => {
    conn.on("open", () => {
      conn.on("data", (data) => {
        let rfle = new File([data.bin], data.name, { type: data.type });

        let url = URL.createObjectURL(
          new Blob([data.bin], { type: data.type }),
        );

        console.log(url);

        recievedFiles = [
          ...recievedFiles,
          { url: url, bin: data.bin, file: rfle },
        ];

        conn.send({ index: data.idx });
      });
    });
  });

  const handleFiles = (e) => {
    files = e.target.files;
  };

  const handleTransfer = () => {

    console.log(address)

    if (files.length > 0 && address) {
      let conn = peer.connect(address);

      const fls = Array.from(files);

      conn.on("open", () => {
        for (const [idx, fi] of fls.entries()) {
          const reader = new FileReader();

          reader.onload = (e) => {
            const arrayBuffer = e.target.result;

            const blob = new Blob([arrayBuffer], { type: fi.type });

            const dat = {
              name: fi.name,
              bin: blob,
              idx: idx,
              type: fi.type,
            };

            conn.send(dat);
          };

          reader.readAsArrayBuffer(fi);
        }

        conn.on("data", (data) => {
          if (data.index) {
            let farr = Array.from(files);

            files = farr.splice(data.index, data.index);

            files = files;
          }
        });
      });
    }
  };

  const handleInput = () => {
    fileInput.click();
  };

  const handleRemove = () => {
    files = [];
    recievedFiles = [];
    files = files;
    recievedFiles = recievedFiles;
  };

  const handleCopy = () => {
    if (id) {
      navigator.clipboard.writeText(id);
    }
  };
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
  />
  <title>FileBazaar</title>
</svelte:head>

<main>
  <input
    type="file"
    multiple
    bind:this={fileInput}
    on:change={handleFiles}
    style="display: none;"
  />
  <Toolbar
    {peer}
    {id}
    bind:address={address}
    {files}
    {recievedFiles}
    {fileInput}
    {handleCopy}
    {handleInput}
    {handleFiles}
    {handleTransfer}
    {handleRemove}
  />
  <Filelist {files} {recievedFiles} />
</main>

<Styles />
