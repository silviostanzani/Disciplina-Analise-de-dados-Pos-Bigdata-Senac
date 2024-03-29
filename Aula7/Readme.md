## Aula7

### Pré=requisito recomedado para acompanhar essa aula: 

* Python https://www.learnpython.org/
* Conda https://gist.github.com/misho-kr/541edc4de148414d83d4bd104f9218f4

### Rede Neural Para Iris (classificação)

#### Fonte:  https://gist.github.com/NiharG15/cd8272c9639941cf8f481a7c4478d525

* Criar ambiente para desenvolvimento de rede neural usando Keras
```
* criar um ambiente conda (conda create -n NN)
* acessar o ambiente conda (source activate NN)
  * * acessar o ambiente conda (No windows)  (conda activate NN)
* Instalar pacote keras (conda install keras)
* Instalar pacote scikit-learn (conda install scikit-learn)
* Instalar Pandas (conda install pandas)
```

* Importando bibliotecas

```
import numpy as np

from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import OneHotEncoder

from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.optimizers import Adam
```

# Preparação de Dados
```
iris_data = load_iris() # load the iris dataset

print('Example data: ')
print(iris_data.data[:5])
print('Example labels: ')
print(iris_data.target[:5])

x = iris_data.data
y_= iris_data.target.reshape(-1, 1) # Convert data to a single column

# Normalização
encoder = OneHotEncoder(sparse=False)
y = encoder.fit_transform(y_)
print(y[:5])

# Dividir em Treino e Teste
train_x, test_x, train_y, test_y = train_test_split(x, y, test_size=0.20)
```

# Construindo o modelo
```
model = Sequential()

model.add(Dense(10, input_shape=(4,), activation='relu', name='fc11'))
model.add(Dense(10, activation='relu', name='fc22'))
model.add(Dense(3, activation='softmax', name='output'))

optimizer = Adam(lr=0.001)
model.compile(optimizer, loss='categorical_crossentropy', metrics=['accuracy'])

print('Neural Network Model Summary: ')
print(model.summary())
```

# Treinando o modelo
```
model.fit(train_x, train_y, verbose=2, epochs=200)
```

# Avaliação do Modelo
```
results = model.evaluate(test_x, test_y)

print('Final test set loss: {:4f}'.format(results[0]))
print('Final test set accuracy: {:4f}'.format(results[1]))

from matplotlib import pyplot as plt

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'val'], loc='upper left')
plt.show()
```

# Predição
* O retorno da predição é uma probabilidade associada a cada classe, e em seguida é feito arredondamento para obter a classe com maior probabilidade
```
test = np.array([[5.1, 3.5, 1.4, 0.2]])
print(test)
ynew = model.predict(test)
print(ynew)
ynew2=np.around(ynew,decimals=1)
print(ynew2)
```

# avaliação
```
from sklearn.metrics import confusion_matrix
test_pred=model.predict(test_x)
test_pred = np.around(test_pred,0)
print(type(test_pred), type(test_y))
test_pred = test_pred.astype(int)
test_y = test_y.astype(int)
confusion_matrix(test_y.argmax(axis=1), test_pred.argmax(axis=1))




```
# Segundo Exemplo - regressão auto - Código Completo
```
from pandas import read_csv
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.wrappers.scikit_learn import KerasRegressor
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import KFold
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline

from sklearn.model_selection import train_test_split
import io
import requests

url = "https://raw.githubusercontent.com/silviostanzani/PosBigData/master/Auto2.csv"
s = requests.get(url).content
dataframe = read_csv(io.StringIO(s.decode('utf-8')))

dataset = dataframe.values

# split into input (X) and output (Y) variables
X = dataset[:,2:8]
y = dataset[:,1]
print(X)
print(y)

train_x, test_x, train_y, test_y = train_test_split(X, y, test_size=0.20)

model = Sequential()
model.add(Dense(20, input_dim=6, kernel_initializer='normal', activation='relu'))
model.add(Dense(1, kernel_initializer='normal'))
# Compile model
model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])

model.fit(train_x, train_y, verbose=2, epochs=200)

results = model.evaluate(test_x, test_y)

print(results)
```

# regressao
```

from pandas import read_csv
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.wrappers.scikit_learn import KerasRegressor
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import KFold
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline

from sklearn.model_selection import train_test_split
import io
import requests
import numpy as np
import requests

url = "https://raw.githubusercontent.com/silviostanzani/PosBigData/master/Auto2.csv"
s = requests.get(url).content
dataframe = read_csv(io.StringIO(s.decode('utf-8')))
print(dataframe)
print(dataframe.columns)
dataset = dataframe.values

# split into input (X) and output (Y) variables
X_ = dataset[:,2:8]
y_ = dataset[:,1]
#print(X)
#print(y)

X = np.asarray(X_).astype(np.float32)
y = np.asarray(y_).astype(np.float32)

train_x, test_x, train_y, test_y = train_test_split(X, y, test_size=0.20)


model = Sequential()

#model.add(Dense(20, input_dim=6, activation='relu'))
#model.add(Dense(1))
model.add(Dense(20, input_dim=6, kernel_initializer='normal', activation='relu'))
model.add(Dense(1, kernel_initializer='normal'))


# Compile model
model.compile(loss='mean_squared_error', optimizer='adam', metrics=['mse'])

history=model.fit(train_x, train_y, verbose=2, epochs=200, validation_split=0.3)


```

Exercício: 

1) No exemplo da Iris diminua o número de neurônios da camada escondida e verifique se a acurácia se modifica
2) Inclua uma camada escondida no exemplo da regressão
3) (OPCIONAL) Instale o miniconda, crie um ambiente virtual com tensorflow e tente rodar um dos modelos acima
