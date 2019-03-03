# DeepRacer コード概要

## デフォルトの構成

```
.
├── common
│   ├── daemon.json
│   ├── docker_utils.py
│   ├── markdown_helper.py
│   ├── misc.py
│   ├── __pycache__
│   │   ├── markdown_helper.cpython-36.pyc
│   │   └── misc.cpython-36.pyc
│   ├── sagemaker_rl
│   │   ├── coach_launcher.py
│   │   ├── configuration_list.py
│   │   ├── docker_utils.py
│   │   ├── __init__.py
│   │   ├── mpi_launcher.py
│   │   ├── onnx_utils.py
│   │   ├── ray_launcher.py
│   │   ├── sage_cluster_communicator.py
│   │   └── stable_baselines_launcher.py
│   └── setup.sh
├── deepracer-hard-track-world.jpg
├── README.md
├── rl_deepracer_coach_robomaker.ipynb
├── src
│   ├── __init__.py
│   ├── markov
│   │   ├── __init__.py
│   │   ├── s3_boto_data_store.py
│   │   ├── s3_client.py
│   │   ├── sagemaker_graph_manager.py
│   │   └── utils.py
│   ├── robomaker
│   │   ├── environments
│   │   │   ├── deepracer_env.py
│   │   │   └── __init__.py
│   │   ├── __init__.py
│   │   └── presets
│   │       ├── deepracer.py
│   │       └── __init__.py
│   └── training_worker.py
└── training.png
```

## メインファイル

```
rl_deepracer_coach_robomaker.ipynb
```

### common配下の読み込むモジュール一覧

```
- sagemaker_rl
- daemon.json
- docker_utils.py
- markdown_helper.py
  - def generate_help_for_s3_endpoint_permissions(role):
  - def generate_help_for_robomaker_trust_relationship(role):
  - def generate_help_for_robomaker_all_permissions(role):
  - def generate_robomaker_links(job_arns, aws_region):
  - def create_s3_endpoint_manually(aws_region, default_vpc):
- misc.py
  - def get_execution_role(role_name="sagemaker", aws_account=None, aws_region=None):
  - def wait_for_s3_object(s3_bucket, key, local_dir, local_prefix='', 
                       aws_account=None, aws_region=None, timeout=1200, limit=20,
                       fetch_only=None, training_job_name=None):
- setup.sh
```

### Setup S3 bucket
- SageMakerのセッション管理インスタンス取得
- S3バケットのセッション管理インスタンス取得
- S3のパスを設定(出力ファイル保存先)

### Define Variables
- SageMaker, RoboMaker の作業インスタンス名を定義
- セッション持続時間を定義
- aws_region定義インスタンスを取得
  -> us-west-2, us-east-1, eu-west-1 以外は許可しない

### Create an IAM role
- SageMakerのロールを取得

### Permission setup for invoking AWS RoboMaker from this notebook
- 設定されている権限を表示する(だけ)

### Configure VPC
- TODO: ごりにまかせる

### Configure the preset for RL algorithm
- src/robomaker/presets/deepracer.py
  のコードを表示
  (モデル開発では、ここを主に変更する)

### Training Entrypoint
- src/training_worker.py
  のソースを読み込む
  モデル学習時のパラメータを変更するためのファイル
  (モデル開発では、ここを主に変更する)

### Train the RL model using the Python SDK Script mode
- S3上のパラメータファイル(deepracer_env.py)を更新する

### metric_definitions
- ClowdWatchのログから、取得したいアルゴリズムメトリクスを定義

### RLEstimator for training RL jobs
- RLジョブパラメータを定義する
- `デフォルトで立ち上がるインスタンスタイプに、無料枠を大きく超えるマシンタイプが指定されているため、必ず書き換えること！`

### Start the Robomaker job
- RoboMakerのジョブを開始する

### Create Simulation Application
- RoboMakerアプリケーションを作成する

### Download the public DeepRaser
- RoboMakerアプリケーションをデプロイする

### Launch the Simulation job on RoboMaker
- RoboMakerシミュレーションジョブを起動する

### Visualizing the simulations in RoboMaker
- RoboMakerで学習状況を可視化する

### Plot metrics for training job
- 学習状況のメトリクスをグラフで可視化する

### Clean Up
- RoboMaker & SageMakerのジョブを終了させる



