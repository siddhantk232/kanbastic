<script>
  let lists = [
    {
      id: "uniqlist1",
      title: "list 1",
      cards: [
        {
          id: "card1",
          title: "card title #1",
          content: "some text that will go into the card",
        },
        {
          id: "card2",
          title: "card title #2",
          content: "some text that will go into the card",
        },
        {
          id: "card3",
          title: "card title #3",
          content: "some text that will go into the card",
        },
      ],
    },
    {
      id: "uniqlist2",
      title: "list 2",
      cards: [],
    },
    {
      id: "uniqlist3",
      title: "list 3",
      cards: [],
    },
  ];

  function ondrag(e) {
    e.dataTransfer.setData("text/plain", e.target.id);
  }

  function ondrop(e) {
    e.preventDefault();
    const data = e.dataTransfer.getData("text/plain");
    const cardId = data.split(" ")[1];
    const listId = data.split(" ")[0];
    const targetId = e.target.id

    // when card drop into the same list
    if (listId === e.target.id) return

    // when cards are dropping on each other
    if (!e.target.id) return

    // exit early if card is dropping on other card
    if (e.target.id.includes("card")) return

    // add card into e.target.id
    const card = lists
      .filter((l) => l.id === listId)[0]
      .cards.filter((c) => c.id === cardId)[0];

    lists = lists.map((l) => {
      if (l.id === targetId) l.cards.push(card);

      if (l.id === listId) l.cards = l.cards.filter((c) => c.id !== cardId);

      return l;
    });

    console.log(lists)
  }

  function dragover(e) {
    e.preventDefault();
    e.dataTransfer.dropEffect = "move";
  }
</script>

<div class="flex space-between">
  {#each lists as list}
    <div
      on:drop={ondrop}
      on:dragover={dragover}
      id={list.id}
      class="flex border border-red-400 flex-1 m-4 flex-col p-8"
    >
      <div class="header bg-gray-200 rounded py-2 px-4">
        <h2>{list.title}</h2>
      </div>

      {#each list.cards as card}
        <div
          draggable="true"
          on:dragstart={ondrag}
          class="p-4 shadow my-2 rounded bg-green-400"
          id="{list.id} {card.id}"
        >
          <h3>{card.title}</h3>
          <p>{card.content}</p>
        </div>
      {/each}
    </div>
  {/each}
</div>
