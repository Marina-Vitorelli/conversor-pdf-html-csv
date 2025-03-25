import streamlit as st
import pdfplumber
import pandas as pd
from bs4 import BeautifulSoup

def pdf_to_html(pdf_file):
    tables = []
    with pdfplumber.open(pdf_file) as pdf:
        for page in pdf.pages:
            extracted_tables = page.extract_tables()
            if extracted_tables:
                tables.extend(extracted_tables)
    html = "<html><body>"
    for table in tables:
        df = pd.DataFrame(table)
        html += df.to_html(index=False, header=False) + "<br>"
    html += "</body></html>"
    return html

def html_to_csv_excel(html_content):
    soup = BeautifulSoup(html_content, "html.parser")
    tables = pd.read_html(str(soup))
    df_final = pd.concat(tables, ignore_index=True)
    return df_final

st.title("Conversor de PDF para HTML + CSV")

uploaded_pdf = st.file_uploader("📄 Envie um PDF", type=["pdf"])

if uploaded_pdf:
    html_result = pdf_to_html(uploaded_pdf)

    if st.button("🔍 Visualizar HTML"):
        st.markdown(html_result, unsafe_allow_html=True)

    if st.button("📥 Baixar CSV / Excel"):
        df_result = html_to_csv_excel(html_result)
        csv = df_result.to_csv(index=False).encode("utf-8")
        excel = df_result.to_excel(index=False, engine='openpyxl')

        st.download_button("⬇️ Baixar CSV", csv, "resultado.csv", "text/csv")
        st.download_button("⬇️ Baixar Excel", excel, "resultado.xlsx", "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
