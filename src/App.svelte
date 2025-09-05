<script>
  // @ts-nocheck

  import logo from "./assets/fbzr_logo_dark_backdrop.png"
  import { v4 as uuidv4 } from "uuid"

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
    Modal,
    Progress,
    ModalHeader,
    Spinner,
    ModalBody,
    ModalFooter,
  } from "@sveltestrap/sveltestrap";

  import { Peer } from "peerjs";
  import Toolbar from "./lib/Toolbar.svelte";
  import Filelist from "./lib/Filelist.svelte";
  import Transfermodal from "./lib/Transfermodal.svelte";
    import Successmodal from "./lib/Successmodal.svelte";

  let peer = new Peer(uuidv4());
  let id;
  let address;
  let open = false;
  let filesSent = 0
  let success = false

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
  
    if (files.length > 0 && address) {

      open = true

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
            filesSent++
          };

          reader.readAsArrayBuffer(fi);
        }

        conn.on("data", (data) => {
          if (data.index) {
            let farr = Array.from(files);

            files = farr.splice(data.index, data.index);

            if (files.length == 0) {
              open = false
              success = true
              filesSent = 0
            }
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
  <link rel="shortcut icon" href={logo} type="image/x-icon">
</svelte:head>

<main>
  <Transfermodal 
    {files} 
    {open} 
    {filesSent} 
  />
  <Successmodal {success} />
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
