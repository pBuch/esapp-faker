# NNÖ ESAPP Faker

Small tool to be used to fake the [ESAPP Personal](https://notrufnoe.com/esapp/) alarm screen for education purposes in
simulation trainings.

Written using [Svelte, Svelte Kit](https://svelte.dev/), [Skeleton-UI](https://www.skeleton.dev/)
and [Lucide](https://lucide.dev/).

Deployed on Github pages: https://pbuch.github.io/esapp-faker/

## Features

- Show ESAPP Alarm screens
- Create, Update, Delete EMS calls
- EMS calls are saved and restored through the browsers LocalStorage
- Import / Export the current collection of calls into/from JSON files

### Known issues

- The name of the EMS call must be unique within the collection, otherwise there are errors upon updating / importing

## Development

Setting up the ESAPP Faker for development is straightforward:

```
git clone https://github.com/pBuch/esapp-faker.git
cd esapp-faker
npm install
npm run dev
```
