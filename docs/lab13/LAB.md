### 🧸️ Lab 13 - Workspace Generators - Intro

###### ⏰ Estimated time: 20-25 minutes

#### 📚 Learning outcomes:

- Understand what workspace generators are
- How to create them
- How to invoke them 
- How to use one to simplify usages of other, more powerful generators

#### 🏋️‍♀️ Steps :

We just learned how important tags are. But we don't want to constantly and manually 
have to maintain them. In this workshop, we'll create a custom workspace generator
called `util-lib` that knows about the folders in our workspace and automatically tags the new project
with a correct scope and type tag.

1. Use the `@nrwl/workspace:workspace-generator` generator to generate a new
workspace generator called `util-lib`

2. Inspect the files that got generated and then commit everything (you'll see in a bit why).

2. Try to run your generator (you can append `--dry-run` to avoid reverting using Git)

   <details>
   <summary>🐳 Hint</summary>

   Inspect the [Workspace generators docs](https://nx.dev/latest/angular/generators/workspace-generators#workspace-generators)
   for details on how to run it.

   </details>
   
   ⚠️ It invokes the `@nrwl/workspace` library generator by default. So it should generate some files when you run it.
   You can use Git to undo those changes (hence why it's recommended to commit before running a generator).

3. In `tools/generators/util-lib/index.ts` try to make it `console.log()` the value of the `--name` property you passed to it (can use `--dry-run` again to test it)

4. Now that we're more familiar with how command line options are passed to the generator,
**let's revert all locally generated files**, as we're about to start making actually useful changes to the generator.

3. The generator should prefix any name you give to your lib with `util-`

   For example:
    - `nx workspace-generator util-lib dates`
    - Should generate a lib with the name `util-dates`

   ⚠️ You can keep trying out your changes safely with the `--dry-run` flag.️

2. Add a new property to its schema called `directory`. It should have only 3 possible values:
`"store", "api", "shared"`. If you do not pass `--directory` as an option when invoking the
schema it should prompt the user to select from the 3 different values (similar to when you got 
asked about which CSS framework to use when creating Angular libs).

   <details>
   <summary>🐳 Hint</summary>

   [Adding dynamic prompts](https://nx.dev/latest/angular/generators/generator-options#adding-dynamic-prompts)

   </details>
    
3. The generator should generate the lib in the directory you pass to it.

4. Because it's a `util` lib, it should automatically be generated with the `type:util` tags.

   <details>
   <summary>🐳 Hint</summary>
   
   Consult the `@nrwl/workspace:lib` [docs](https://nx.dev/latest/angular/workspace/library)
   for possible options you can pass to it.

   </details>

5. We also need to add `scope` tag to it. We can use the `directory` value for this, since it signifies our scope.

6. Before testing your changes, remember to commit them, in case you need to revert
locally generated files again.

4. Invoke your generator and test if the above requirements work
    - Let's give it the name `notifications`
    - Select `api` as the directory

6. Let's add some functionality to the lib you just created:
    - In `libs/api/util-notifications/src/lib/api-util-notifications.ts`
    - Add:
   
        ```
       export function sendNotification(clientId: string) {
         console.log("sending notification to client: ", clientId);
       }
       ```

7. Now try to import the above function in `apps/api/src/app/app.service.ts`
    - Try to lint all the apps
    - It should work because everything is in the `api` scope
    
8. Try to import it in `apps/store/src/app/app.component.ts`
    - It should fail because it's not within the same scope
    
9. In `libs/api/util-notifications/src/lib/api-util-notifications.ts`
    - Try to import a `feature` lib
    - It should correctly fail because the type hierarchy is not respected
    
10. **BONUS -** Currently, the schema passed to the generator is of type `any` - `tools/generators/util-lib/index.ts`
Try to create an interface for it that respects the generator's schema.

11. **BONUS BONUS** - try to create another similar generator, but for "feature" libs.
12. Commit everything before the next lab.

---

🎓If you get stuck, check out [the solution](SOLUTION.md)

---

[➡️ Next lab ➡️](../lab14/LAB.md)
