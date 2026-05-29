# Save Item To List

1. Open any shopping list on Sam's Club
2. Press F12 (or whatever you press to open the console on your browser) 
3. Paste the below code in, and you'll see the SPC is added to your list.

``` javascript
(async () => {
  const productId = "19170800669";
  const qty = 2;

  const cookies = document.cookie.split("; ");
  const tokenCookie = cookies.find(entry => entry.startsWith("authToken="));
  const token = tokenCookie ? tokenCookie.split("=")[1] : null;

  const pathMatch = location.pathname.match(/\/lists\/[^/]+\/([\w-]+)/);
  const list = pathMatch ? pathMatch[1] : null;

  if (!token) {
    console.error("You aren't logged in.");
    return;
  }

  if (!list) {
    console.error("You must be on a shopping list page for this to work.");
    return;
  }

  const endpoint =
    "https://www.samsclub.com/orchestra/lists/graphql/addItemToListLite/9ba1253e682b673b597549601a886e80538592e8035f3e182ddb294b66342da5";

  const body = {
    variables: {
      input: {
        items: [
          {
            usItemId: productId,
            quantity: qty,
            itemType: "REGULAR",
          },
        ],
        listId: list,
      },
      name: "",
      type: "WL",
      ownerName: "",
      showPrimary: false,
      maxItemsReached: false,
      enablePromotionMessages: true,
    },
  };

  console.log(`Adding item ${productId} to Sam's Club list...`);

  try {
    const res = await fetch(endpoint, {
      method: "POST",
      credentials: "include",
      headers: {
        accept: "application/json",
        authorization: decodeURIComponent(token),
        "content-type": "application/json",
        "tenant-id": "gj9b60",
        "x-apollo-operation-name": "addItemToListLite",
        "x-o-bu": "SAMS-US",
        "x-o-mart": "B2C",
        "x-o-platform": "rweb",
        "x-o-segment": "oaoh",
      },
      body: JSON.stringify(body),
    });

    const data = await res.json();

    if (data.errors) {
      console.error("Item could not be added:", data.errors);
    } else {
      console.log("Item added successfully:", data);
    }
  } catch (problem) {
    console.error("Unexpected error:", problem);
  }
})();
```