
import cv2
import numpy as np

def process_image(image):
    # 1. Grayscale Conversion
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

    # 2. Gaussian Blur
    blur = cv2.GaussianBlur(gray, (5, 5), 0)

    # 3. Canny Edge Detection
    edges = cv2.Canny(blur, 50, 150)

    # 4. Region of Interest
    height, width = edges.shape
    mask = np.zeros_like(edges)
    polygon = np.array([[(0, height), (width, height), (width // 2, height // 2)]], np.int32)
    cv2.fillPoly(mask, polygon, 255)
    masked_edges = cv2.bitwise_and(edges, mask)

    # 5. Hough Transform
    lines = cv2.HoughLinesP(masked_edges, 2, np.pi/180, 100, np.array([]), minLineLength=40, maxLineGap=5)

    # 6. Line Averaging and Drawing (This part can be more complex)
    # ... (code to average and extrapolate lines)

    return line_image # The image with the detected lanes drawn on it
