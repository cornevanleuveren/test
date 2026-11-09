# Get Aspected running

Instructions for an AI assistant (Claude Code, Claude, or any AI that can use a terminal). A person pointed you here because they want to try Aspected on their own machine, with their own data.

Aspected is a vector database that ranks records on several weighted aspects at once (meaning, numbers, categories, dates) and shows the distance per aspect for every result. Queries can leave aspects open: only the aspects you ask about count.

**Source of truth:** https://docs.aspected.com. If anything on this page disagrees with the docs, follow the docs and tell the person.

## Rules

- Ask the person before you install anything (Docker, images, models).
- Keep their data on their machine.
- Do not invent commands, endpoints or fields. Check the docs, starting with Getting Started and the Tutorial.
- Stop and ask when the docs and their setup disagree.

## Steps

1. Check that Docker is installed: `docker --version`. If it is not, ask the person to install Docker Desktop first.
2. Pull and run Aspected:

   ```
   docker pull xillio/aspected:latest
   docker run -p 8080:8080 xillio/aspected:latest
   ```

   With a GPU and text embeddings in mind, see Getting Started for the `vulkan-latest` and `cuda-latest` images.
3. Check that it responds: `curl http://localhost:8080/indexes`. Expect `"status": "OK"` and zero indexes.
4. Ask the person for data: a CSV or Excel file. If they have none, use a small public CSV and say which one.
5. Profile the columns and propose a schema: one aspect per useful column, each with a resolver from the docs: `enum` for a fixed list of values, `number`, `datetime`, or `text` for descriptions (text needs a GGUF model; see the Text resolver page). Show the list and wait for their OK.
6. Create the index and upload the rows, as in the Tutorial (`PUT /indexes/{name}`, then `POST /indexes/{name}/docs`).
7. Run three "like this, but" searches, for example "like this one, but cheaper", leaving the other aspects open. Show each result with its distance per aspect.
8. Change a weight in the query (`"weights": {"<aspect>": 2.0}`), run the same search again and show what moved and why.
9. Show them Aspected Studio at http://localhost:8080/studio.
10. End with a short summary: what you built, the commands you ran, and how their own app can call the REST API.

## If something fails

- Port 8080 is taken: run with `-p 8081:8080` and use port 8081.
- A feature says it needs a license (for example the dictionary resolver or document access control): see Getting Started, "Activating your license".
- Stuck, or the person wants help on their own data: https://aspected.com/contact-form
