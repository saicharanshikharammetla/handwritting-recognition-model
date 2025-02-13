 { useEffect, useRef, useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";

export default function HandwritingRecognition() {
  const canvasRef = useRef(null);
  const [recognizedText, setRecognizedText] = useState("");

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.strokeStyle = "black";
    ctx.lineWidth = 4;
    ctx.lineCap = "round";

    let drawing = false;

    const startDrawing = (e) => {
      drawing = true;
      ctx.beginPath();
      ctx.moveTo(e.offsetX, e.offsetY);
    };

    const draw = (e) => {
      if (!drawing) return;
      ctx.lineTo(e.offsetX, e.offsetY);
      ctx.stroke();
    };

    const stopDrawing = () => {
      drawing = false;
      ctx.closePath();
    };

    canvas.addEventListener("mousedown", startDrawing);
    canvas.addEventListener("mousemove", draw);
    canvas.addEventListener("mouseup", stopDrawing);
    canvas.addEventListener("mouseleave", stopDrawing);

    return () => {
      canvas.removeEventListener("mousedown", startDrawing);
      canvas.removeEventListener("mousemove", draw);
      canvas.removeEventListener("mouseup", stopDrawing);
      canvas.removeEventListener("mouseleave", stopDrawing);
    };
  }, []);

  const handleRecognize = async () => {
    const canvas = canvasRef.current;
    const imageData = canvas.toDataURL("image/png");
    // Send imageData to OCR model for recognition (placeholder function)
    const recognized = await recognizeHandwriting(imageData);
    setRecognizedText(recognized);
  };

  const recognizeHandwriting = async (image) => {
    // Placeholder for OCR integration
    return "42"; // Mock result
  };

  return (
    <Card className="p-4 max-w-lg mx-auto mt-5">
      <CardContent>
        <canvas ref={canvasRef} width={400} height={200} className="border rounded" />
        <div className="flex gap-2 mt-4">
          <Button onClick={handleRecognize}>Recognize</Button>
        </div>
        {recognizedText && <p className="mt-2">Recognized Text: {recognizedText}</p>}
      </CardContent>
    </Card>
  );
}
cognition-model
