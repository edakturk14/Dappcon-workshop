# Dappcon workshop 2024

1. Go to: https://scaffoldeth.io/

2. npx create-eth@latest

^ follow the instructions on the wizard 

Your project name: dappcon
What solidity framework do you want to use? hardhat OR foundry
Install packages? Yes

3. cd dappcon
  
- Start the local development node: yarn chain
- In a new terminal window, deploy your contracts: yarn deploy
- In a new terminal window, start the frontend: yarn start

4. Open the project in your code editor: two folders here 
-  hardhat/foundry
-  nextjs

You are able to make small changes to your smart contract and redeploy it by running `yarn deploy` again. You can directly interact on the updates UI. 

5. Add a new updateOwner function 

```sol
function updateOwner(address _newOwner) public {
    owner = _newOwner;
}
```

add the isOwner modifier to the updateOwner function

6. Go to the deploy script and change the ownwer address to your frontend. 

Deploy the contract again: yarn deploy

7. Now lets edit the frontend -- go to nextjs/app/pages.tsx

Head over to the daisy ui docs: https://daisyui.com/components/

Let's first delete the existing code and only show the connected address. We'll use the account component of scaffold eth. 

First display the connected address (Conneceted adress: {connectedAddress}) and then format it with se2. 

```tsx

"use client";

import Link from "next/link";
import type { NextPage } from "next";
import { useAccount } from "wagmi";
import { BugAntIcon, MagnifyingGlassIcon } from "@heroicons/react/24/outline";
import { Address } from "~~/components/scaffold-eth";

const Home: NextPage = () => {
  const { address: connectedAddress } = useAccount();

  return (
    <>
      <div className="flex items-center flex-col flex-grow pt-10">
        connectedAddress: <Address address={connectedAddress} />
      </div>
    </>
  );
};

export default Home;

```

8. Now lets add a card to have to make it prettier. 

```tsx
  return (
    <>
      <div className="flex items-center flex-col flex-grow pt-10">
        <div className="card w-96 bg-base-100 shadow-xl">
          <div className="card-body">
            <h2 className="card-title">Update Owner</h2>
            <Address address={connectedAddress} />
            <div className="card-actions justify-end">
              <button className="btn btn-primary">Update owner</button>
            </div>
          </div>
        </div>
      </div>
    </>
  );
```

9. Get the owner address from the contract 
- you can use wagmi use contract read 
- you can use se-2 hooks

```tsx
  const { data: contractOwner } = useScaffoldReadContract({
    contractName: "YourContract",
    functionName: "owner",
  });
```

10. Now lets bring in an AddressInput to let the use add a n address and update to it. 

const [newAddress, setNewAddress] = useState<string>("");

you'll need to import react and then add the addressInput to the page

<AddressInput value={newAddress} onChange={setNewAddress} />

11. Now lets add a button to make the change 

<button className="btn w-full btn-primary" onClick={() => { }}>
 Update Owner
</button>

12. We need to add some functionality to the button in order to update the owner. 
- you can use wagmi contract write 
- you can use se2 hooks 

```tsx
            <button
              className="btn btn-primary"
              onClick={async () => {
                try {
                  await writeYourContractAsync({
                    functionName: "updateOwner",
                    args: [newAddress],
                  });
                } catch (e) {
                  console.error("Error changing owner:", e);
                }
              }}
            >
              Update owner
            </button>
```

