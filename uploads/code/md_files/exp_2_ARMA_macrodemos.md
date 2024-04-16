<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap">
</head>

<body>

<div style="background-color: #65BF6F; color: black; text-align: center; padding: 10px; font-family: 'Roboto', sans-serif; font-size: 24px">
    

<div style="text-align:center;">
    <img src="Jaki_Charrua_gatodelespacio.jpg" alt="Drawing" style="width:150px; border-radius:90%;"/>
    
    
</div> 


<body>


    
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://use.typekit.net/your-typekit-id.css">
</head>
<body>
    
    
<div style="background-color: #000000; color: white; text-align: center; padding: 10px; font-family: 'Proxima Nova', sans-serif; font-size: 24px;">
    Replication Lab  <br>
    GEC0319 | Time Series <br>
    
    
</div>



    
    
</body>
</html>


```python
# !pip install macrodemos
import macrodemos
```


```python
# help(macrodemos)
```


```python
macrodemos.ARMA()
```



<iframe
    width="100%"
    height="650"
    src="http://127.0.0.1:8050/"
    frameborder="0"
    allowfullscreen

></iframe>



    Dash app running on http://127.0.0.1:8050/
    


```python
macrodemos.Markov('state 0', 'state 1')  # define states for your own application
```

    C:\Users\juani\anaconda3\envs\series_de_tiempo\lib\site-packages\dash\dash.py:539: UserWarning:
    
    JupyterDash is deprecated, use Dash instead.
    See https://dash.plotly.com/dash-in-jupyter for more details.
    
    


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp\ipykernel_18652\585710223.py in <module>
    ----> 1 macrodemos.Markov('state 0', 'state 1')  # define states for your own application
    

    ~\anaconda3\envs\series_de_tiempo\lib\site-packages\macrodemos\demo_Markov.py in Markov(colab, *states)
        279     else:
        280         webbrowser.open('http://127.0.0.1:8050/')
    --> 281         app.run_server(debug=False)
        282 
        283 Markov_demo = Markov
    

    ~\anaconda3\envs\series_de_tiempo\lib\site-packages\jupyter_dash\jupyter_app.py in run_server(
        self,
        mode,
        width,
        height,
        inline_exceptions,
        **kwargs
    )
        220         old_server = self._server_threads.get((host, port))
        221         if old_server:
    --> 222             old_server.kill()
        223             old_server.join()
        224             del self._server_threads[(host, port)]
    

    ~\anaconda3\envs\series_de_tiempo\lib\site-packages\jupyter_dash\_stoppable_thread.py in kill(self)
         14         thread_id = self.get_id()
         15         res = ctypes.pythonapi.PyThreadState_SetAsyncExc(
    ---> 16             ctypes.c_long(thread_id), ctypes.py_object(SystemExit)
         17         )
         18         if res == 0:
    

    TypeError: an integer is required (got type NoneType)

