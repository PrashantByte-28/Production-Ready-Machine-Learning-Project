# VisaSight — MLOps Pipeline for US Visa Approval Prediction

This is a machine learning project I built to predict whether a US work visa
petition is likely to get approved or denied, based on things like the
employee's education, job experience, wage, and the employer's company size.

I didn't want to just train a model in a notebook and leave it there, so I
built this as a full pipeline — data ingestion, validation, transformation,
training, evaluation — and then wrapped it in a FastAPI app so you can
actually fill out a form and get a live prediction. It's also deployed with
Docker on AWS EC2, with GitHub Actions handling the CI/CD.

Dataset used: [EasyVisa dataset on Kaggle](https://www.kaggle.com/datasets/moro23/easyvisa-dataset)

## What it does

You fill out a short form (continent, education, experience, wage, company
size, etc.) and the model tells you whether it thinks the petition would be
approved or not. Models used are XGBoost and CatBoost — I tried a few things
and these gave the best results on this dataset.

For monitoring, I added Evidently AI so I could check for data drift later
on, since that's something that gets skipped a lot in smaller projects but
matters in a real setup.

## Stack

- Python 3.8
- Scikit-learn, XGBoost, CatBoost
- MongoDB (for storing the raw data)
- FastAPI + Jinja2 for the web app
- Docker
- AWS EC2 + ECR
- GitHub Actions (self-hosted runner on the EC2 instance)

## How the pipeline is structured

I followed a fairly standard modular structure so each part can be tested
and run on its own:

```
constants -> entity -> components -> pipeline -> app.py
```

- **constants** – fixed config stuff (paths, DB names, thresholds)
- **entity** – config/artifact classes passed between stages
- **components** – ingestion, validation, transformation, training, evaluation
- **pipeline** – wires the components together (training pipeline + prediction pipeline)
- **app.py** – FastAPI app that loads the trained model and serves predictions

## Running it locally

```bash
git clone https://github.com/PrashantByte-28/VisaSight.git
cd VisaSight
# (repo renamed from Production-Ready-Machine-Learning-Project)

conda create -n visa python=3.8 -y
conda activate visa
pip install -r requirements.txt
```

You'll need a MongoDB connection string and AWS keys set as environment
variables before running anything:

```bash
export MONGODB_URL="mongodb+srv://<username>:<password>@..."
export AWS_ACCESS_KEY_ID=<your_key>
export AWS_SECRET_ACCESS_KEY=<your_secret>
```

Train the model:

```bash
python demo.py
```

Then run the app:

```bash
python app.py
```

and go to `localhost:8080` in your browser.

## Deployment

This part took me the longest to get right, honestly. The flow is:

```
push to GitHub -> GitHub Actions builds a Docker image
-> pushes it to ECR -> EC2 pulls the image -> restarts the container
```

Steps I followed to set it up:

1. Created an IAM user with `AmazonEC2ContainerRegistryFullAccess` and `AmazonEC2FullAccess`
2. Created an ECR repo to hold the image
3. Spun up an EC2 instance (Ubuntu) and installed Docker on it:
   ```bash
   sudo apt-get update -y && sudo apt-get upgrade -y
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   sudo usermod -aG docker ubuntu
   newgrp docker
   ```
4. Registered that same EC2 instance as a self-hosted GitHub Actions runner
   (Settings → Actions → Runners → New self-hosted runner)
5. Added these as repo secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`,
   `AWS_DEFAULT_REGION`, `ECR_REPO`

After that, every push to `main` rebuilds and redeploys automatically —
no manual steps.

## Things I'd still like to improve

- Add proper unit tests for the pipeline components (currently none, which I know isn't great)
- Track experiments with MLflow instead of just comparing metrics manually
- Handle edge cases in the form better on the frontend
- Set up scheduled drift checks instead of only checking manually

## Author

Prashant Kumar Mishra
[GitHub](https://github.com/PrashantByte-28) · [LinkedIn](https://www.linkedin.com/in/prashant-kumar-mishra-356004261)
