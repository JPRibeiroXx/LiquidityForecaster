
# Bank Liquidity Forecaster

This project is a machine learning-based tool for forecasting daily liquidity in banks. It combines data preprocessing, feature engineering, and advanced machine learning techniques to predict financial metrics based on historical data.

---

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/JPRibeiroXx/LiquidityForecaster.git
   cd LiquidityForecaster
   ```

2. **Set up the Python Environment**:
   - Use Python 3.8 or newer.
   - Install dependencies:
     ```bash
     pip install -r requirements.txt
     ```

3. **Prepare the Dataset**:
   - Place the raw data files (`bancos_python.csv` and `rubricas_python.csv`) in the `data/` folder.

---

## Usage

### 1. Run the Forecasting Notebook

- Open the `Forecasting.ipynb` notebook in Jupyter Notebook or Jupyter Lab.
- Follow the steps to:
  - Preprocess the data.
  - Train and evaluate the model.
  - Forecast liquidity for future periods.

### 2. Preprocessing Overview

**Input Data**:

- `bancos_python.csv`: Contains bank transactions with columns for dates, values (`Valor`), and transaction types (`BNCDoc`).
- `rubricas_python.csv`: Maps transaction types to broader categories (`Rubrica`).

**Processing Steps**:

- Parse dates and set them as the index.
- Clean the `Valor` column (remove symbols, convert to numerical).
- Map transaction types (`BNCDoc`) to categories (`Rubrica`).
- Remove irrelevant categories (e.g., `"Transferencias_internas"`).
- Create a pivot table to aggregate `Rubrica` values by day.

### 3. Training the Model

- Split the data into training and testing sets based on a forecasting horizon.
- Use XGBoost regression with optimized hyperparameters for prediction.
- Evaluate model performance with RMSE metrics.

### 4. Forecasting Future Data

- Extend the dataset with synthetic rows for the forecast period.
- Generate predictions using the trained model.
- Visualize predicted liquidity trends.

---

## Example Workflow

### Load the data:

```python
bank = pd.read_csv('bancos_python.csv')
rubrica = pd.read_csv('rubricas_python.csv', index_col=[0]).T.to_dict(orient='records')
```

### Preprocess the data:

```python
bank['Data'] = pd.to_datetime(bank['Data'], format='%d/%m/%Y')
bank.set_index('Data', inplace=True)
bank['Rubrica'] = bank['BNCDoc'].map(rubrica[0])
bank.loc[bank['BNCDoc'] == 'abertura', 'Rubrica'] = 'Abertura'
bank.drop('BNCDoc', axis=1, inplace=True)
bank['Valor'] = bank['Valor'].str.split(' ').str[0].str.replace(',', '').astype(float)
bank = bank[bank['Rubrica'] != 'Transferencias_internas']
```

### Forecast liquidity:

- Train the model on the cleaned dataset.
- Use lagged features and recent historical data for forecasting.
- Save the model for reuse.

---

## Contributions

Contributions are welcome! Please fork the repository and submit a pull request.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE.md) file for details.

---

## Acknowledgements

Special thanks to João S. Ribeiro for developing this tool.
