#!/usr/bin/env python
# coding: utf-8

# In[1]:


import pandas as pd


# # Load data

# In[2]:


data=pd.read_csv('source.csv')


# In[3]:


data.head()


# # Describe the numerical columns

# In[4]:


data.describe()


# # Get information about dataset and columns' datatype

# In[5]:


data.info()


# # Change Datetime column datatype from object to datetime

# In[6]:


data['Datetime']=pd.to_datetime(data['Datetime'],utc=True)


# In[7]:


data['Datetime']


# # Conver UTC timezone into UTC+6

# In[8]:


data['Datetime'] = data['Datetime'].dt.tz_convert('Asia/Almaty')


# In[9]:


data['Datetime']


# # Create a dictionary to store Product A prices by timestamp for the diff calculation

# In[12]:


product_a_prices = {}
for index, row in data[data['Name'] == 'ProductA'].iterrows():
    product_a_prices[row['Datetime']] = row['Price']


# # Function to calculate the total based on the rules

# In[13]:


def calculate_total(row):
    if row['Name'] == 'ProductA':
        price = row['Price'] * (0.75 if row['Purity'] == 'Impure' else 1)
        return row['Amount'] * price
    elif row['Name'] == 'ProductB':
        # Get the corresponding Product A price for the same timestamp
        product_a_price = product_a_prices.get(row['Datetime'], 0)
        price_diff = row['Price'] - product_a_price
        adjusted_price = price_diff * (0.75 if row['Purity'] == 'Impure' else 1)
        return row['Amount'] * adjusted_price
    return 0


# # Apply the calculation to each row

# In[15]:


data['Total'] = data.apply(calculate_total, axis=1)


# # Save result as a csv file

# In[18]:


data.to_csv('result.csv', index=False)


# In[ ]:




