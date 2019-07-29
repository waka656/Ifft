# フーリエ変換・逆変換をリアルタイムに処理するプログラム

〇プログラムの説明

  - OpenCVで画像ファイルを読み込む(im)
  - 読み込んだ画像と同じサイズの配列im2,im3,im4を0で初期化して宣言
  - 画像をフーリエ変換し,中心が低周波になるように処理して配列fshiftに格納
  - それぞれの関数では以下の処理を行う.
    - draw_orbit:マウスの軌道を描く画像を作成する関数

      OpenCVによって,マウスの左ボタンが押されている状態の時に処理を行う
      - マウスポインタの指す座標が画像サイズ内に収まるように調整する
      - マウスポインタの指す座標(x,y)に対応するim2の成分に255を代入（画像上で白で表示される）
      - ifft1,ifft2を(x,y)で呼び出す

    - ifft1:マウスの軌道上の周波数を記録してフーリエ逆変換する関数

      - マウスポインタの指す座標の周波数成分をim3の対応する座標に代入
      - im3をフーリエ逆変換する(im3_back)
      - im3_backの実数成分のみ抽出

      ifft1ではマウスポインタの軌跡上の座標に対応する周波数成分がすべて反映されている.
    - ifft2:マウスポインタ上の周波数を記録してフーリエ逆変換する関数

      - im4を0で初期化
      - マウスポインタの指す座標の周波数成分をim4の対応する座標に代入
      - im4をフーリエ逆変換する(im4_back)
      - im4_backの実数成分のみ抽出

      ifft2では,関数の初めにim4を0で初期化しているため,関数が呼び出された時点でのマウスポインタの周波数成分のみが反映されている.
  - 'C'キーが押されるまで以下の処理を行う

    - ウィンドウ"Pointer"上のマウスイベントをdraw_orbitで読み込む
    - ウィンドウ"Pointer"にim2を出力
    - ウィンドウ"Ifft"にim3_backを出力
    - ウィンドウ"Signal_Pointer"にim4_backを出力
    - ウィンドウ"Original"にimを出力
    - ウィンドウ"Magnitude_Spectrum"にmagnitude_spectrumを出力

  - すべてのウィンドウを閉じる

〇実装した処理
  - 周波数領域の画像上のマウスポインタの座標に対して以下の画像を生成
    - マウスポインタの軌跡(im2)
    - その座標における空間領域での波形の画像(im4_back)
    - マウスポインタの軌跡上の周波数を基に再構成した空間領域での画像(im3_back)

〇プログラム作成時に注意した点
  - 読み込んだ画像ファイルが大きかったため,サイズを小さくして処理を行った
  - OpenCVで画像出力を行うために,画像の型をuint8に変換した
  - マウスポインタの座標が画像サイズの範囲を超えないようにした

〇使い方
  - プログラムを実行する
  - ウィンドウ"Pointer"上でマウスをドラッグすると,マウスの軌跡に合わせて処理された画像が各ウィンドウに出力される

〇実行の仕方

  JupyterNotebook上でプログラムを実行する.

〇実行時の動作

<img src="https://vps10-d.kuku.lu/files/20190729-0143_01ba0e0d830da7b6d6b070dd3af70313.gif" width=70%>

※ウィンドウ名が表示されていないが,一番左が"Pointer",上左が"Original",上右が"Magnitude_Spectrum",下左が"Ifft",下右が"Signal_Pointer"である.

〇依存ライブラリとバージョン
  - cv2(バージョン4.1.0)
  - numpy(バージョン1.11.3)
  - matplotlib.pyplot
  - skimage.io
  - skimage.color

〇参考にしたサイト
  - OpenCV-Python Tutorials 1 documentation/フーリエ変換(http://lang.sist.chukyo-u.ac.jp/classes/OpenCV/py_tutorials/py_imgproc/py_transforms/py_fourier_transform/py_fourier_transform.html)

    Numpyを使ったフーリエ変換のコードを参考にした.
  - OpenCV-Python Tutorials 1 documentation/ペイントツールとしてのマウス(http://labs.eecs.tottori-u.ac.jp/sd/Member/oyamada/OpenCV/html/py_tutorials/py_gui/py_mouse_handling/py_mouse_handling.html)

  cv2.setMouseCallback()の使い方を参考にした.
  - OpenCV/ユーザインタフェース(http://opencv.jp/opencv-2svn/cpp/highgui_user_interface.html)

  OpenCVでのimshow()の使い方を参考にした.

  - 無能プログラマーのお勉強おメモ/PythonとOpenCVで画像処理④【マウスイベント】(http://rasp.hateblo.jp/entry/2016/01/24/204539)

  マウスイベントの関数を参考にした.
