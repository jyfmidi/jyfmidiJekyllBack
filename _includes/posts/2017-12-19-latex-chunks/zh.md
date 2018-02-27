 
## 图片
#### 插入图片
```
\usepackage{float}
\usepackage{graphicx}
...
\begin{figure}[H]
    \centering
    \includegraphics[width=7cm]{1.png}
\end{figure}
```

#### 图文并列
```
\usepackage{wrapfig}
...
\begin{wrapfigure}{r}{0.3\textwidth} % Inline image example
	\begin{center}
		\includegraphics[width=0.3\textwidth]{1.png}
	\end{center}
	\caption{picture name}
\end{wrapfigure}
Sentences besides the picture
```

## 三线表
```
begin{table}[!hbp]
\centering
\renewcommand\arraystretch{1.5}
\begin{tabular}{ll}
\hline
符号 & 说明\\
\hline
$v$ & 顶点坐标 \\
$vt$ & 纹理坐标\\
$vn$ & 法向量\\
$f$ & 面上点/纹理/法向量组\\
\hline
\end{tabular}
\end{table}
```

## 饮用
tex 同目录创建 sample.bib 作为文献库，格式如下
```
@BOOK{ArbRaised,
	title = {Course in general linguistics},
	publisher = {Jiuzhou Press},
	author = {Ferdinand de Saussure},
	year = {2007},
	volume={1},
	edition = {1st},
	pages={155},
}

@ARTICLE{levelico,
    lang={Chinese},
	author = {Chunguang Lu},
	title = {Iconicity in Linguistic Signs},
	journal = {Foreign Language Teaching Theory and Practice},
	year = {2008},
	volume={1},
	publisher = {Anhui Literature Press}
}
```
在 .tex 文件中加入
```
% import package in header
\usepackage{cite}

...
Some sentences\cite{ArbRaised}. Other sentences\cite{levelico}
...
% place for bibliography chunk
\bibliographystyle{unsrt}
\bibliography{sample}

```
