import os
from flask import Flask, request, jsonify

app = Flask(__name__)

UPLOAD_FOLDER = 'uploads'
OUTPUT_FOLDER = 'output'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
os.makedirs(OUTPUT_FOLDER, exist_ok=True)

@app.route('/merge', methods=['POST'])
def merge_pdfs():
    # Check if files are uploaded
    if 'files' not in request.files:
        return jsonify({'error': 'No files uploaded'}), 400
    
    files = request.files.getlist('files')
    if len(files) < 2:
        return jsonify({'error': 'Upload at least two PDF files'}), 400

    # Save uploaded files
    file_paths = []
    for file in files:
        if file.filename.endswith('.pdf'):
            file_path = os.path.join(UPLOAD_FOLDER, file.filename)
            file.save(file_path)
            file_paths.append(file_path)
        else:
            return jsonify({'error': f'{file.filename} is not a PDF'}), 400

    # Merge PDFs using mutool
    output_file = os.path.join(OUTPUT_FOLDER, 'merged.pdf')
    merge_command = ['mutool', 'merge', output_file] + file_paths
    result = os.system(' '.join(merge_command))

    if result == 0:
        return jsonify({'message': 'PDFs merged successfully', 'output': output_file}), 200
    else:
        return jsonify({'error': 'PDF merging failed'}), 500

if __name__ == '__main__':
    app.run(debug=True)
